# Modbus协议

### 环境准备：

1. 安装模拟器。poll和slave
1. 安装虚拟串口驱动vspd
2. modbus设备有编号slaveID
3. function（存储区）01coil status（输出线圈）和03holding register（输出寄存器）为读写 02 input status（输入线圈）和 04 input register（输入寄存器）为只读 01是bool类型 03是ushort类型
4. address是存储地址指的是从第几号内存开始编码0-65535
5. quantity是数量
6. 模拟串口选择serialport模拟网口则选择其他4个区别是报文区别
7. 串口选择对于设备来说是从COM2->COM1 （COM2和COM1一定是成对出现）
8. slave是上位机poll是下位机

### 几种不同模式

1. （推荐）RTU模式:串口通讯。编码二进制CRC校验（循环冗余校验）
2. ASCII模式：串口通讯。编码ASCIILRC校验（纵向冗余校验）
3. （推荐）TCP：网口通讯
4. UDP：网口通讯

### 报文格式

小于3.5个字符的报文间隔时间 +从站地址1* byte +功能码1 * byte + 数据位N * bytes + 校验位 2 * bytes 

### 存储区

输出线圈 bool 读写 0区

输入线圈 bool 只读 1区

输入寄存器 寄存器类型 只读 3区

输出寄存器 寄存器类型 读写 4区

### 功能码

1. 读取输出线圈 0x01
2. 读取输入线圈 0x02
3. 读取输入寄存器 0x03
4. 读取输出寄存器 0x04
5. 写单个线圈 0x05
6. 写多个线圈 0x0F
7. 写单个寄存器 0x06
8. 写多个寄存器 0x10

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

   
