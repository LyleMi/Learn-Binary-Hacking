Type Confusion
=================================

Desc
---------------------------------

Description Summary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The program allocates or initializes a resource such as a pointer, object, or variable using one type, but it later accesses that resource using a type that is incompatible with the original type. 

Extended Description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
When the program accesses the resource using an incompatible type, this could trigger logical errors because the resource does not have expected properties. In languages without memory safety, such as C and C++, type confusion can lead to out-of-bounds memory access.

While this weakness is frequently associated with unions when parsing data with many different embedded object types in C, it can be present in any application that can interpret the same variable or memory location in multiple ways.

This weakness is not unique to C and C++. For example, errors in PHP applications can be triggered by providing array parameters when scalars are expected, or vice versa. Languages such as Perl, which perform automatic conversion of a variable of one type when it is accessed as if it were another type, can also contain these issues.

Example
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