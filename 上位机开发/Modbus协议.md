# Modbus协议

### 环境准备：

1. 安装模拟器。poll和slave
1. 安装虚拟串口驱动vspd
2. modbus设备有编号slaveID
3. function（存储区）01coil status和03holding register为读写 02 input status和 04 input register为只读 01是bool类型 03是ushort类型
4. address是存储地址指的是从第几号内存开始编码0-65535
5. quantity是数量
6. 模拟串口选择serialport模拟网口则选择其他4个区别是报文区别
7. 串口选择对于设备来说是从COM2->COM1 （COM2和COM1一定是成对出现）
8. slave是上位机poll是下位机

### 代码部分：

1. 安装NModbus4，System.IO.Ports包

2. 在C＃中， **ushort**是一个关键字，用于声明一个变量，该变量可以存储介于**0到65,535**之间的无符号整数值。 **ushort关键字**是System.UInt16的别名。

3. 数据都是ushort []

4. ```c#
               //创建串口连接
               SerialPort port = new SerialPort();
               //确定通信方式
               port.PortName = "COM1";
               port.BaudRate = 9600;
               port.DataBits = 8;
               port.StopBits = StopBits.One;
               port.Parity = Parity.None;
               port.Open();
               var master = ModbusSerialMaster.CreateRtu(port);
              var data= master.ReadHoldingRegisters(1,0,20);
               Console.WriteLine(data[0]);
             var str=  Console.ReadLine();
               
               Console.WriteLine("写入中。。。");
               master.WriteSingleRegister(1,0,Convert.ToUInt16(str));
               Console.WriteLine("写入成功。。。");
               //线圈状态
               Console.WriteLine("读取线圈状态。。。");
               var bool_data = master.ReadCoils(1, 0, 1);
               Console.WriteLine(bool_data[0]);
               Console.WriteLine("写入线圈状态。。。");
               master.WriteSingleCoil(1,0,! bool_data[0]!);
   ```

5. 如果是使用网口通讯.使用ModbusTcp包

6. ```c#
               ModbusClient client = new ModbusClient("127.0.0.1",502);
               client.Init();
              
                       var data =client.ReadRegistersAsync(0,2).Result;
                       Console.WriteLine(data[0] + "" + data[1]);
   ```

   
