# C#导入Excel

```c#
/// <summary>
        /// 导入Excel
        /// </summary>
        /// <returns></returns>
        [HttpPost("ImportExcel")]
        [Auth(SysUserRole.Admin, true)]
        [Log("ImportExcel")]
        public OperateResponse ImportExcel() {
            
                return Excute(() =>
                {
                var files = Request.Form.Files;
                var file = files.FirstOrDefault();
                if (file == null)
                {
                    return Fail("");
                }
                string ReturnValue = string.Empty;
                //定义一个bool类型的变量用来做验证
                bool flag = true;

                    try
                    {
                        string fileExt = Path.GetExtension(file.FileName).ToLower();
                        //定义一个集合一会儿将数据存储进来,全部一次丢到数据库中保存
                        var Data = new List<SysUser>();
                        MemoryStream ms = new MemoryStream();
                        file.CopyTo(ms);
                        ms.Seek(0, SeekOrigin.Begin);
                        IWorkbook book;
                        if (fileExt == ".xlsx")
                        {
                            book = new XSSFWorkbook(ms);
                        }
                        else if (fileExt == ".xls")
                        {
                            book = new HSSFWorkbook(ms);
                        }
                        else
                        {
                            book = null;
                        }
                        ISheet sheet = book.GetSheetAt(0);

                        int CountRow = sheet.LastRowNum + 1;//获取总行数

                        if (CountRow - 1 == 0)
                        {
                            return Fail("Excel列表数据项为空!");
                        }
                        #region 循环验证
                        for (int i = 1; i < CountRow; i++)
                        {
                            //获取第i行的数据
                            var row = sheet.GetRow(i);
                            if (row != null)
                            {
                                //循环的验证单元格中的数据
                                for (int j = 0; j < 5; j++)
                                {
                                    if (row.GetCell(j) == null || row.GetCell(j).ToString().Trim().Length == 0)
                                    {
                                        flag = false;
                                        ReturnValue += $"第{i + 1}行,第{j + 1}列数据不能为空。";
                                    }
                                }
                            }
                        }
                        #endregion
                        if (flag)
                        {
                            for (int i = 1; i < CountRow; i++)
                            {
                                //实例化实体对象
                                SysUser user = new SysUser();
                                var row = sheet.GetRow(i);

                                if (row.GetCell(0) != null && row.GetCell(0).ToString().Trim().Length > 0)
                                {
                                    user.Name = row.GetCell(0).ToString();
                                }
                                if (row.GetCell(1) != null && row.GetCell(1).ToString().Trim().Length > 0)
                                {
                                    user.UserName = row.GetCell(1).ToString();
                                }
                                if (row.GetCell(2) != null && row.GetCell(2).ToString().Trim().Length > 0)
                                {
                                    user.Password = EncryptPwd(row.GetCell(2).ToString());
                                }
                                if (row.GetCell(3) != null && row.GetCell(3).ToString().Trim().Length > 0)
                                {
                                    user.Phone = row.GetCell(3).ToString();
                                }
                                if (row.GetCell(4) != null && row.GetCell(4).ToString().Trim().Length > 0)
                                {
                                    user.CompID = new CompanyDomain().GetModel(e => e.Name == row.GetCell(4).ToString())?.ID ?? 0;
                                }
                                if (row.GetCell(5) != null && row.GetCell(5).ToString().Trim().Length > 0)
                                {
                                    var t = row.GetCell(5).ToString();
                                    switch (t)
                                    {
                                        case "村(社)环卫负责人":
                                            user.UserType = SysUserRole.SanWorker;
                                            break;
                                        case "执法中队队员":
                                            user.UserType = SysUserRole.LawWorker;
                                            break;
                                        case "管理员":
                                            user.UserType = SysUserRole.Admin;
                                            break;
                                        case "执法中队管理员":
                                            user.UserType = SysUserRole.LawEnForcement;
                                            break;
                                        case "环卫公司管理员":
                                            user.UserType = SysUserRole.SanitationMan;
                                            break;
                                        case "村巡察员":
                                            user.UserType = SysUserRole.Patroler;
                                            break;
                                        default:
                                            user.UserType = SysUserRole.Other;
                                            break;
                                        
                                    }
                                    /*user.UserType = EnumHelper.ToEnum<SysUserRole>(row.GetCell(5).ToString(), SysUserRole.Other);*/
                                }


                                Data.Add(user);
                            }

                        }
                        else
                        {
                            return Fail(ReturnValue);
                        }
                        var userdomain = new SysUserDoMain();
                        userdomain.AddRange(Data);
                        return Success();
                    }
                    catch (Exception)
                    {

                        return Fail("Excel格式有误"+ReturnValue);
                    }
                    
                    
                });
        
        }
```

