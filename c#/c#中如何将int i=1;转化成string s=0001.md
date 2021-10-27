# c#中如何将int i=1;转化成string s="0001"

1.string s = i.ToString("0000");
2.i.ToString().PadLeft(4, '0');
3.i.ToString("d4");