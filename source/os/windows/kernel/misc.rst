其他
========================================

字符
----------------------------------------
Windows在内核中以UTF-16LE的格式处理字符串。由于许多程序都使用ANSI字符串，因此Windows在大部分函数中提供了支持ANSI和Unicode的两个接口。例如CreateFile实际对应CreateFileA(ANSI)和CreateFileW (Wide, unicode) 两个函数。

其中对unicode的原生支持是在9x版本后才引入的。
