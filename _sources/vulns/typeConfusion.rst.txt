Type Confusion
=================================

简介
---------------------------------
类型混淆的问题发生在程序使用一种类型分配或初始化诸如指针，对象或变量之类的资源，但在之后使用与原始类型不兼容的类型访问该资源的情况。

当程序使用不兼容类型访问资源时，可能会触发逻辑错误，因为资源没有预期的属性。在没有内存安全性的语言中，例如C和C++，类型混淆可能导致越界内存访问。

虽然在C中使用许多不同的嵌入对象类型解析数据时，这种漏洞经常与 ``union`` 相关联，但它可以存在于可以以多种方式解释相同变量或内存位置的任何应用程序中。

这个漏洞并不是C和C++独有的。PHP应用程序中的一些漏洞可以通过在期望标量时提供数组参数来触发，反之亦然。像Perl这样的语言，当变量被执行自动转换时，也可能包含这些问题。

示例
---------------------------------

::

    #define NAME_TYPE 1
    #define ID_TYPE 2

    struct MessageBuffer
    {
        int msgType;
        union {
            char *name;
            int nameID;
        };
    };


    int main (int argc, char **argv) {
        struct MessageBuffer buf;
        char *defaultMessage = "Hello World";

        buf.msgType = NAME_TYPE;
        buf.name = defaultMessage;
        printf("Pointer of buf.name is %p\n", buf.name);
        /* This particular value for nameID is used to make the code architecture-independent. If coming from untrusted input, it could be any value. */
        buf.nameID = (int)(defaultMessage + 1);
        printf("Pointer of buf.name is now %p\n", buf.name);
        if (buf.msgType == NAME_TYPE) {
            printf("Message: %s\n", buf.name);
        }
        else {
            printf("Message: Use ID %d\n", buf.nameID);
        }
    }
