.. _ref-specification:

Specification Files
===================

A SIP specification consists of some C/C++ type and function declarations and
some :ref:`directives <ref-directives>`.  The declarations may contain
:ref:`annotations <ref-annotations>` which provide SIP with additional
information that cannot be expressed in C/C++.  SIP does not implement a full
C/C++ parser.


Syntax Definition
-----------------

The following is a semi-formal description of the syntax of a specification
file.

.. parsed-literal::
    *specification* ::= {*module-statement*}

    *module-statement* ::= [*module-directive* | *statement*]

    *module-directive* ::= [
            :directive:`%CompositeModule` |
            :directive:`%Copying` |
            :directive:`%DefaultDocstringFormat` |
            :directive:`%DefaultDocstringSignature` |
            :directive:`%DefaultEncoding` |
            :directive:`%DefaultMetatype` |
            :directive:`%DefaultSupertype` |
            :directive:`%ExportedHeaderCode` |
            :directive:`%ExportedTypeHintCode` |
            :directive:`%Extract` |
            :directive:`%Feature` |
            :directive:`%HideNamespace` |
            :directive:`%Import` |
            :directive:`%Include` |
            :directive:`%InitialisationCode` |
            :directive:`%License` |
            :directive:`%MinimumABIVersion` |
            :directive:`%Module` |
            :directive:`%ModuleCode` |
            :directive:`%ModuleHeaderCode` |
            :directive:`%Platforms` |
            :directive:`%PreInitialisationCode` |
            :directive:`%PostInitialisationCode` |
            :directive:`%Timeline` |
            :directive:`%TypeHintCode` |
            :directive:`%UnitCode` |
            :directive:`%UnitPostIncludeCode` |
            :directive:`%VirtualErrorHandler` |
            *mapped-type* |
            *mapped-type-template*]

    *statement* :: [*class-statement* | *function* | *variable*]

    *class-statement* :: [
            :directive:`%If` |
            *class* |
            *class-template* |
            *enum* |
            *namespace* |
            *opaque-class* |
            *operator* |
            *struct* |
            *union* |
            *typedef* |
            *exception*]

    *class* ::= **class** *name* [**:** *super-classes*] [*class-annotations*]
            **{** {*class-line*} **};**

    *super-classes* ::= [**public** | **protected** | **private**] *name*
            [**,** *super-classes*]

    *class-line* ::= [
            *class-statement* |
            :directive:`%BIGetBufferCode` |
            :directive:`%BIReleaseBufferCode` |
            :directive:`%ConvertFromTypeCode` |
            :directive:`%ConvertToSubClassCode` |
            :directive:`%ConvertToTypeCode` |
            :directive:`%Docstring` |
            :directive:`%FinalisationCode` |
            :directive:`%GCClearCode` |
            :directive:`%GCTraverseCode` |
            :directive:`%InstanceCode` |
            :directive:`%PickleCode` |
            :directive:`%TypeCode` |
            :directive:`%TypeHeaderCode` |
            :directive:`%TypeHintCode` |
            *constructor* |
            *destructor* |
            *method* |
            *static-method* |
            *virtual-method* |
            *special-method* |
            *operator* |
            *virtual-operator* |
            *class-variable* |
            **public:** |
            **public Q_SLOTS:** |
            **public slots:** |
            **protected:** |
            **protected Q_SLOTS:** |
            **protected slots:** |
            **private:** |
            **private Q_SLOTS:** |
            **private slots:** |
            **Q_SIGNALS:** |
            **signals:**]

    *constructor* ::= [**explicit**] *name* **(** [*argument-list*] **)**
            [**noexcept**] [*function-annotations*]
            [*c++-constructor-signature*] **;** [:directive:`%Docstring`]
            [:directive:`%MethodCode`]

    *c++-constructor-signature* ::= **[(** [*argument-list*] **)]**

    *destructor* ::= [**virtual**] **~** *name* **()** [**noexcept**] [**= 0**]
            [*function-annotations*] **;** [:directive:`%MethodCode`]
            [:directive:`%VirtualCatcherCode`]

    *method* ::= [**Q_SIGNAL**] [**Q_SLOT**] *type* *name* **(**
            [*argument-list*] **)** [**const**] [**final**] [**noexcept**]
            [**= 0**] [*function-annotations*] [*c++-signature*] **;**
            [:directive:`%Docstring`] [:directive:`%MethodCode`]

    *c++-signature* ::= **[** *type* **(** [*argument-list*] **)]**

    *static-method* ::= **static** *function*

    *virtual-method* ::= [**Q_SIGNAL**] [**Q_SLOT**] **virtual** *type* *name*
            **(** [*argument-list*] **)** [**const**] [**final**]
            [**noexcept**] [**= 0**] [*function-annotations*] [*c++-signature*]
            **;** [:directive:`%MethodCode`] [:directive:`%VirtualCatcherCode`]
            [:directive:`%VirtualCallCode`]

    *special-method* ::= *type* *special-method-name*
            **(** [*argument-list*] **)** [*function-annotations*] **;**
            [:directive:`%MethodCode`]

    *special-method-name* ::= [**__abs__** | **__add__** | **__and__** |
            **__aiter__** | **__anext__** | **__await__** | **__bool__** |
            **__call__** | **__contains__** | **__delattr__** |
            **__delitem__** | **__div__** | **__eq__** | **__float__** |
            **__floordiv__** | **__ge__** | **__getattr__** |
            **__getattribute__** | **__getitem__** | **__gt__** |
            **__hash__** | **__iadd__** | **__iand__** | **__idiv__** |
            **__ifloordiv__** | **__ilshift__** | **__imatmul__** |
            **__imod__** | **__imul__** | **__index__** | **__int__** |
            **__invert__** | **__ior__** | **__irshift__** | **__isub__** |
            **__iter__** | **__itruediv__** | **__ixor__** | **__le__** |
            **__len__** | **__lshift__** | **__lt__** | **__matmul** |
            **__mod__** | **__mul__** | **__ne__** | **__neg__** |
            **__next__** | **__or__** | **__pos__** | **__repr__** |
            **__rshift__** | **__setattr__** | **__setitem__** | **__str__** |
            **__sub__** | **__truediv__** | **__xor__**]

    *operator* ::= *operator-type*
            **(** [*argument-list*] **)** [**const**] [**final**]
            [**noexcept**] [*function-annotations*] **;**
            [:directive:`%MethodCode`]

    *virtual-operator* ::= **virtual** *operator-type*
            **(** [*argument-list*] **)** [**const**] [**final**]
            [**noexcept**] [**= 0**] [*function-annotations*] **;**
            [:directive:`%MethodCode`] [:directive:`%VirtualCatcherCode`]
            [:directive:`%VirtualCallCode`]

    *operatator-type* ::= [ *operator-function* | *operator-cast* ]

    *operator-function* ::= *type* **operator** *operator-name*

    *operator-cast* ::= **operator** *type*

    *operator-name* ::= [**+** | **-** | ***** | **/** | **%** | **&** |
            **|** | **^** | **<<** | **>>** | **+=** | **-=** | ***=** |
            **/=** | **%=** | **&=** | **|=** | **^=** | **<<=** | **>>=** |
            **~** | **()** | **[]** | **<** | **<=** | **==** | **!=** |
            **>** | **>>=** | **=**]

    *class-variable* ::= [**static**] *variable*

    *class-template* :: = **template** **<** *type-list* **>** *class*

    *mapped-type* :: = :directive:`%MappedType` *base-type*
            [*mapped-type-annotations*]
            **{** {*mapped-type-line*} **};**

    *mapped-type-line* ::= [
            :directive:`%If` |
            :directive:`%ConvertFromTypeCode` |
            :directive:`%ConvertToTypeCode` |
            :directive:`%ReleaseCode` |
            :directive:`%TypeCode` |
            :directive:`%TypeHeaderCode` |
            *enum* |
            *function* |
            *variable*]

    *mapped-type-template* :: = **template** **<** *type-list* **>**
            *mapped-type*

    *enum* ::= **enum** [*enum-key*] [*name*] [**:** *enum-base*]
            [*enum-annotations*] **{** {*enum-line*} **};**

    *enum-key* ::= [**class** | **struct**]

    *enum-base* ::= *base-type*

    *enum-line* ::= [:directive:`%If` | *name* [*enum-annotations*] **,**

    *function* ::= *typed-name* **(** [*argument-list*] **)** [**noexcept**]
            [*function-annotations*] **;** [:directive:`%Docstring`]
            [:directive:`%MethodCode`]

    *namespace* ::= **namespace** *name* [**{** {*namespace-line*} **}**] **;**

    *namespace-line* ::= [
            :directive:`%Docstring` |
            :directive:`%TypeHeaderCode` |
            *statement*]

    *opaque-class* ::= **class** *scoped-name* **;**

    *struct* ::= **struct** *name* **{** {*class-line*} **};**

    *union* ::= **union** *name* **{** {*class-line*} **};**

    *typedef* ::= **typedef** [*typed-name* | *function-pointer*]
            *typedef-annotations* **;**

    *variable*::= *typed-name* [*variable-annotations*] **;**
            [:directive:`%AccessCode`] [:directive:`%GetCode`]
            [:directive:`%SetCode`]

    *exception* ::= :directive:`%Exception` *exception-name* [*exception-base*]
            **{** [:directive:`%TypeHeaderCode`] :directive:`%RaiseCode` **};**

    *exception-name* ::= *scoped-name*

    *exception-base* ::= **(** [*exception-name* | *python-exception*] **)**

    *python-exception* ::= [**SIP_ArithmeticError** | **SIP_AssertionError** |
            **SIP_AttributeError** | **SIP_BaseException** |
            **SIP_BlockingIOError** | **SIP_BrokenPipeError** |
            **SIP_BufferError** | **SIP_ChildProcessError** |
            **SIP_ConnectionAbortedError** | **SIP_ConnectionError** |
            **SIP_ConnectionRefusedError** | **SIP_ConnectionResetError** |
            **SIP_EnvironmentError** | **SIP_EOFError** | **SIP_Exception** |
            **SIP_FileExistsError** | **SIP_FileNotFoundError** |
            **SIP_FloatingPointError** | **SIP_GeneratorExit** |
            **SIP_ImportError** | **SIP_IndentationError** |
            **SIP_IndexError** | **SIP_InterruptedError** | **SIP_IOError** |
            **SIP_IsADirectoryError** | **SIP_KeyboardInterrupt** |
            **SIP_KeyError** | **SIP_LookupError** | **SIP_MemoryError** |
            **SIP_NameError** | **SIP_NotADirectoryError** |
            **SIP_NotImplementedError** | **SIP_OSError** |
            **SIP_OverflowError** | **SIP_PermissionError** |
            **SIP_ProcessLookupError** | **SIP_ReferenceError** |
            **SIP_RuntimeError** | **SIP_StandardError** |
            **SIP_StopIteration** | **SIP_SyntaxError** | **SIP_SystemError** |
            **SIP_SystemExit** | **SIP_TabError** | **SIP_TimeoutError** |
            **SIP_TypeError** | **SIP_UnboundLocalError** |
            **SIP_UnicodeDecodeError** | **SIP_UnicodeEncodeError** |
            **SIP_UnicodeError** | **SIP_UnicodeTranslateError** |
            **SIP_ValueError** | **SIP_VMSError** | **SIP_WindowsError** |
            **SIP_ZeroDivisionError** | **SIP_Warning** |
            **SIP_BytesWarning** | **SIP_DeprecationWarning** |
            **SIP_FutureWarning** | **SIP_ImportWarning** |
            **SIP_PendingDeprecationWarning** | **SIP_ResourceWarning** |
            **SIP_RuntimeWarning** | **SIP_SyntaxWarning** |
            **SIP_UnicodeWarning** | **SIP_UserWarning**]

    *argument-list* ::= *argument* [**,** *argument-list*] [**,** **...**]

    *argument* ::= *type* [*name*] [*argument-annotations*] [*default-value*]

    *default-value* ::= **=** *expression*

    *expression* ::= [*value* | *value* *binary-operator* *expression*]

    *value* ::= [*unary-operator*] *simple-value*

    *simple-value* ::= [*scoped-name* | *function-call* | *real-value* |
            *integer-value* | *boolean-value* | *string-value* |
            *character-value*]

    *typed-name*::= *type* *name*

    *function-pointer*::= *type* **(*** *name* **)(** [*type-list*] **)**

    *type-list* ::= *type* [**,** *type-list*]

    *function-call* ::= *scoped-name* **(** [*value-list*] **)**

    *value-list* ::= *value* [**,** *value-list*]

    *real-value* ::= a floating point number

    *integer-value* ::= a number

    *boolean-value* ::= [**true** | **false**]

    *string-value* ::= **"** {*character*} **"**

    *character-value* ::= **'** *character* **'**

    *unary-operator* ::= [**!** | **~** | **-** | **+** | **\*** | **&**]

    *binary-operator* ::= [**-** | **+** | ***** | **/** | **&** | **|**]

    *argument-annotations* ::= see :ref:`ref-arg-annos`

    *class-annotations* ::= see :ref:`ref-class-annos`

    *enum-annotations* ::= see :ref:`ref-enum-annos`

    *function-annotations* ::= see :ref:`ref-function-annos`

    *mapped-type-annotations* ::= see :ref:`ref-mapped-type-annos`

    *typedef-annotations* ::= see :ref:`ref-typedef-annos`

    *variable-annotations* ::= see :ref:`ref-variable-annos`

    *type* ::= [**const**] *base-type* {*****} [**&**]

    *type-list* ::= *type* [**,** *type-list*]

    *base-type* ::= [*scoped-name* | *template* |
            **struct** *scoped-name* |
            **union** *scoped-name* |
            **char** | **signed char** | **unsigned char** | **wchar_t** |
            **int** | **unsigned** | **unsigned int** | **size_t** |
            **short** | **unsigned short** |
            **long** | **unsigned long** |
            **long long** | **unsigned long long** |
            **float** | **double** |
            **bool** |
            **void** |
            **Py_hash_t** |
            **Py_ssize_t** |
            **PyObject** |
            :stype:`SIP_PYBUFFER` |
            :stype:`SIP_PYCALLABLE` |
            :stype:`SIP_PYDICT` |
            :stype:`SIP_PYENUM` |
            :stype:`SIP_PYLIST` |
            :stype:`SIP_PYOBJECT` |
            :stype:`SIP_PYSLICE` |
            :stype:`SIP_PYTUPLE` |
            :stype:`SIP_PYTYPE`]

    *scoped-name* ::= *name* [**::** *scoped-name*]

    *template* ::= *scoped-name* **<** *type-list* **>**

    *dotted-name* ::= *name* [**.** *dotted-name*]

    *name* ::= _A-Za-z {_A-Za-z0-9}

Here is a short list of differences between C++ and the subset supported by
SIP that might trip you up.

    - SIP does not support the use of ``[]`` in types.  Use pointers instead.

    - A global ``operator`` can only be defined if its first argument is a
      class or a named enum that has been wrapped in the same module.

    - Variables declared outside of a class are effectively read-only.


Variable Numbers of Arguments
-----------------------------

SIP supports the use of ``...`` as the last part of a function signature.  Any
remaining arguments are collected as a Python tuple.


Additional SIP Types
--------------------

SIP supports a number of additional data types that can be used in Python
signatures.

.. sip-type:: SIP_PYBUFFER

This is a ``PyObject *`` that implements the Python buffer protocol.


.. sip-type:: SIP_PYCALLABLE

This is a ``PyObject *`` that is a Python callable object.


.. sip-type:: SIP_PYDICT

This is a ``PyObject *`` that is a Python dictionary object.


.. sip-type:: SIP_PYENUM

This is a ``PyObject *`` that is a Python enum object.


.. sip-type:: SIP_PYLIST

This is a ``PyObject *`` that is a Python list object.


.. sip-type:: SIP_PYOBJECT

This is a ``PyObject *`` of any Python type.  The type ``PyObject *`` can also
be used.


.. sip-type:: SIP_PYSLICE

This is a ``PyObject *`` that is a Python slice object.


.. sip-type:: SIP_PYTUPLE

This is a ``PyObject *`` that is a Python tuple object.


.. sip-type:: SIP_PYTYPE

This is a ``PyObject *`` that is a Python type object.


Python API vs. C/C++ API
------------------------

It is important to understand that a SIP specification describes the Python
API, i.e. the API available to the Python programmer when they ``import`` the
generated module.  It does not have to accurately represent the underlying
C/C++ library.  There is nothing wrong with omitting functions that make
little sense in a Python context, or adding functions implemented with
handwritten code that have no C/C++ equivalent.  It is even possible (and
sometimes necessary) to specify a different super-class hierarchy for a C++
class.  All that matters is that the generated code compiles properly.

In most cases the Python API matches the C/C++ API.  In some cases handwritten
code (see :directive:`%MethodCode`) is used to map from one to the other
without SIP having to know the details itself.  However, there are a few cases
where SIP generates a thin wrapper around a C++ method or constructor (see
:ref:`ref-derived-classes`) and needs to know the exact C++ signature.  To deal
with these cases SIP allows two signatures to be specified.  For example::

    class Klass
    {
    public:
        // The Python signature is a tuple, but the underlying C++ signature
        // is a 2 element array.
        Klass(SIP_PYTUPLE) [(int *)];
    %MethodCode
            int iarr[2];

            if (PyArg_ParseTuple(a0, "ii", &iarr[0], &iarr[1]))
            {
                // Note that we use the SIP generated derived class
                // constructor.
                Py_BEGIN_ALLOW_THREADS
                sipCpp = new sipKlass(iarr);
                Py_END_ALLOW_THREADS
            }
    %End
    };


Namespaces
----------

SIP implements C++ namespaces as a Python class which cannot be instantiated.
The contents of the namespace, including nested namespaces, are implemented as
attributes of the class.

The namespace class is created in the module that SIP is parsing when it first
sees the namespace defined.  If a function (for example) is defined in a
namespace that is first defined in another module then the function is added to
the namespace class in that other module.

Say that we have a file ``a.sip`` that defines a module ``a_module`` as
follows::

    %Module a_module

    namespace N
    {
        void hello();
    };

We also have a file ``b.sip`` that defines a module ``b_module`` as follows::

    %Module b_module

    %Import a.sip

    namespace N
    {
        void bye();
    };

When SIP parses ``b.sip`` it first sees the ``N`` namespace defined in module
``a_module``.  Therefore it places the ``bye()`` function in the ``N`` Python
class in the ``a_module``.  It does not create an ``N`` Python class in the
``b_module``.  Consequently the following code will call the ``bye()``
function::

    import a_module
    import b_module
    a_module.N.bye()

While this reflects the C++ usage it may not be obvious to the Python
programmer who might expect to call the ``bye()`` function using::

    import b_module
    b_module.N.bye()

In order to achieve this behavior make sure that the ``N`` namespace is first
defined in the ``b_module``.  The following version of ``b.sip`` does this::

    %Module b_module

    namespace N;

    %Import a.sip

    namespace N
    {
        void bye();
    };

Alternatively you could just move the :directive:`%Import` directive so that it
is at the end of the file.
