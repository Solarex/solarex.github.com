---
layout: post
title: "Java Reflection"
date: 2014-09-16 06:54
comments: true
categories: 
- dev
- java
---

[java reflect tutorial](http://docs.oracle.com/javase/tutorial/reflect/)

Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine.Reflection is powerful, but should not be used indiscriminately. If it is possible to perform an operation without using reflection, then it is preferable to avoid using it. The following concerns should be kept in mind when accessing code via reflection.

+ ``Performance Overhead``:Because reflection involves types that are dynamically resolved, certain Java virtual machine optimizations can not be performed. Consequently, reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.
+ ``Security Restrictions``:Reflection requires a runtime permission which may not be present when running under a security manager. This is in an important consideration for code which has to run in a restricted security context, such as in an Applet.
+ ``Exposure of Internals``:Since reflection allows code to perform operations that would be illegal in non-reflective code, such as accessing private fields and methods, the use of reflection can result in unexpected side-effects, which may render code dysfunctional and may destroy portability. Reflective code breaks abstractions and therefore may change behavior with upgrades of the platform.

<!-- more -->

## Classes

Every object is either a ``reference`` or ``primitive type``. Reference types all inherit from ``java.lang.Object``. ``Classes``, ``enums``, ``arrays``, and ``interfaces`` are all reference types. There is a fixed set of primitive types: ``boolean, byte, short, int, long, char, float, and double``. Examples of reference types include ``java.lang.String``, all of the wrapper classes for primitive types such as ``java.lang.Double``, the interface ``java.io.Serializable``, and the enum ``javax.swing.SortOrder``.

The entry point for all reflection operations is ``java.lang.Class``. With the exception of ``java.lang.reflect.ReflectPermission``, none of the classes in ``java.lang.reflect`` have public constructors. To get to these classes, it is necessary to invoke appropriate methods on Class. There are several ways to get a Class depending on whether the code has access to an object, the name of class, a type, or an existing Class.

+ ``Object.getClass()``

If an instance of an object is available, then the simplest way to get its Class is to invoke ``Object.getClass()``. Of course, this only works for reference types which all inherit from ``Object``. Some examples follow.``Class c = "foo".getClass();``Returns the Class for ``String``,``Class c = System.console().getClass();``There is a unique console associated with the virtual machine which is returned by the static method ``System.console()``. The value returned by ``getClass()`` is the Class corresponding to ``java.io.Console``.

```java
enum E { A, B }
Class c = A.getClass();
```

A is is an instance of the enum ``E``; thus ``getClass()`` returns the Class corresponding to the enumeration type ``E``.

```java
byte[] bytes = new byte[1024];
Class c = bytes.getClass();
```

Since arrays are Objects, it is also possible to invoke ``getClass()`` on an instance of an array. The returned Class corresponds to an array with component type byte.

```java
import java.util.HashSet;
import java.util.Set;

Set<String> s = new HashSet<String>();
Class c = s.getClass();
```

In this case, ``java.util.Set`` is an interface to an object of type ``java.util.HashSet``. The value returned by ``getClass()`` is the class corresponding to ``java.util.HashSet``.

+ The ``.class`` Syntax

If the type is available but there is no instance then it is possible to obtain a Class by appending ".class" to the name of the type. This is also the easiest way to obtain the Class for a **primitive type**.

```java
boolean b;
Class c = b.getClass();   // compile-time error

Class c = boolean.class;  // correct
```

Note that the statement ``boolean.getClass()`` would produce a compile-time error because a ``boolean`` is a primitive type and cannot be dereferenced. The ``.class`` syntax returns the Class corresponding to the type boolean.``Class c = java.io.PrintStream.class;``The variable c will be the Class corresponding to the type ``java.io.PrintStream``.``Class c = int[][][].class;``The ``.class`` syntax may be used to retrieve a Class corresponding to a multi-dimensional array of a given type.

+ ``Class.forName()``

If the fully-qualified name of a class is available, it is possible to get the corresponding Class using the static method ``Class.forName()``. This cannot be used for primitive types. The syntax for names of array classes is described by ``Class.getName()``. **This syntax is applicable to references and primitive types**.``Class c = Class.forName("com.duke.MyLocaleServiceProvider");``This statement will create a class from the given fully-qualified name.``Class cDoubleArray = Class.forName("[D");`` ``Class cStringArray = Class.forName("[[Ljava.lang.String;");``
The variable ``cDoubleArray`` will contain the Class corresponding to an array of primitive type double (i.e. the same as ``double[].class``). The ``cStringArray`` variable will contain the Class corresponding to a two-dimensional array of String (i.e. identical to ``String[][].class``).


+ ``TYPE`` Field for Primitive Type Wrappers

The ``.class`` syntax is a more convenient and the preferred way to obtain the Class for a primitive type; however there is another way to acquire the Class. Each of the **primitive types and void** has a wrapper class in ``java.lang`` that is used for boxing of primitive types to reference types. Each wrapper class contains a field named ``TYPE`` which is equal to the Class for the primitive type being wrapped.``Class c = Double.TYPE;``There is a class ``java.lang.Double`` which is used to wrap the primitive type double whenever an Object is required. The value of ``Double.TYPE`` is identical to that of ``double.class``.``Class c = Void.TYPE;`` ``Void.TYPE`` is identical to ``void.class``.

+ Methods that Return Classes

There are several Reflection APIs which return classes but these may only be accessed if a Class has already been obtained either directly or indirectly.

``Class.getSuperclass()``Returns the super class for the given class.``Class c = javax.swing.JButton.class.getSuperclass();``The super class of ``javax.swing.JButton`` is ``javax.swing.AbstractButton``.

``Class.getClasses()``Returns all the public classes, interfaces, and enums that are members of the class including inherited members.``Class<?>[] c = Character.class.getClasses();`` ``Character`` contains two member classes ``Character.Subset`` and ``Character.UnicodeBlock``.

``Class.getDeclaredClasses()``Returns all of the classes interfaces, and enums that are explicitly declared in this class.``Class<?>[] c = Character.class.getDeclaredClasses();``Character contains two public member classes ``Character.Subset`` and ``Character.UnicodeBlock`` and one private class ``Character.CharacterCache``.

```java
Class.getDeclaringClass()
java.lang.reflect.Field.getDeclaringClass()
java.lang.reflect.Method.getDeclaringClass()
java.lang.reflect.Constructor.getDeclaringClass()
```

Returns the Class in which these members were declared. Anonymous Class Declarations will not have a declaring class but will have an enclosing class.

```java
import java.lang.reflect.Field;

Field f = System.class.getField("out");
Class c = f.getDeclaringClass();
```

The field ``out`` is declared in ``System``.


```java
public class MyClass {
    static Object o = new Object() {
        public void m() {} 
    };
    static Class<c> = o.getClass().getEnclosingClass();
}
```

The declaring class of the anonymous class defined by o is ``null``.

``Class.getEnclosingClass()``Returns the immediately enclosing class of the class.``Class c = Thread.State.class().getEnclosingClass();``The enclosing class of the enum ``Thread.State`` is ``Thread``.

```java
public class MyClass {
    static Object o = new Object() { 
        public void m() {} 
    };
    static Class<c> = o.getClass().getEnclosingClass();
}
```

The anonymous class defined by o is enclosed by MyClass.


A class may be declared with one or more modifiers which affect its runtime behavior:

+ Access modifiers: public, protected, and private
+ Modifier requiring override: abstract
+ Modifier restricting to one instance: static
+ Modifier prohibiting value modification: final
+ Modifier forcing strict floating point behavior: strictfp
+ Annotations

Not all modifiers are allowed on all classes, for example an interface cannot be final and an enum cannot be abstract. ``java.lang.reflect.Modifier`` contains declarations for all possible modifiers. It also contains methods which may be used to decode the set of modifiers returned by ``Class.getModifiers()``.

```java
import java.lang.annotation.Annotation;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import java.lang.reflect.TypeVariable;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import static java.lang.System.out;

public class ClassDeclarationSpy {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());
	    out.format("Modifiers:%n  %s%n%n",
		       Modifier.toString(c.getModifiers()));

	    out.format("Type Parameters:%n");
	    TypeVariable[] tv = c.getTypeParameters();
	    if (tv.length != 0) {
		out.format("  ");
		for (TypeVariable t : tv)
		    out.format("%s ", t.getName());
		out.format("%n%n");
	    } else {
		out.format("  -- No Type Parameters --%n%n");
	    }

	    out.format("Implemented Interfaces:%n");
	    Type[] intfs = c.getGenericInterfaces();
	    if (intfs.length != 0) {
		for (Type intf : intfs)
		    out.format("  %s%n", intf.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Implemented Interfaces --%n%n");
	    }

	    out.format("Inheritance Path:%n");
	    List<Class> l = new ArrayList<Class>();
	    printAncestor(c, l);
	    if (l.size() != 0) {
		for (Class<?> cl : l)
		    out.format("  %s%n", cl.getCanonicalName());
		out.format("%n");
	    } else {
		out.format("  -- No Super Classes --%n%n");
	    }

	    out.format("Annotations:%n");
	    Annotation[] ann = c.getAnnotations();
	    if (ann.length != 0) {
		for (Annotation a : ann)
		    out.format("  %s%n", a.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Annotations --%n%n");
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printAncestor(Class<?> c, List<Class> l) {
	Class<?> ancestor = c.getSuperclass();
 	if (ancestor != null) {
	    l.add(ancestor);
	    printAncestor(ancestor, l);
 	}
    }
}
```

Not all annotations are available via reflection. Only those which have a ``java.lang.annotation.RetentionPolicy`` of ``RUNTIME`` are accessible. Of the three annotations pre-defined in the language ``@Deprecated``, ``@Override``, and ``@SuppressWarnings`` only ``@Deprecated`` is available at runtime.

## Members

Reflection defines an interface ``java.lang.reflect.Member`` which is implemented by ``java.lang.reflect.Field``, ``java.lang.reflect.Method``, and ``java.lang.reflect.Constructor``. According to The Java Language Specification, Java SE 7 Edition, the members of a class are the inherited components of the class body including ``fields``, ``methods``, ``nested classes``, ``interfaces``, and ``enumerated types``. Since constructors are not inherited, they are not ``members``. This differs from the implementing classes of ``java.lang.reflect.Member``.

### Fields

Fields have a type and a value. The ``java.lang.reflect.Field`` class provides methods for accessing type information and setting and getting values of a field on a given object.A field is a class, interface, or enum with an associated value. Methods in the ``java.lang.reflect.Field`` class can retrieve information about the field, such as its name, type, modifiers, and annotations.

When writing an application such as a class browser, it might be useful to find out which fields belong to a particular class. A class's fields are identified by invoking ``Class.getFields()``. The ``getFields()`` method returns an array of Field objects containing one object per accessible public field.

A public field is accessible if it is a member of either:

+ this class
+ a superclass of this class
+ an interface implemented by this class
+ an interface extended from an interface implemented by this class

A field may be a class (instance) field, such as ``java.io.Reader.lock`` , a static field, such as ``java.lang.Integer.MAX_VALUE`` , or an enum constant, such as ``java.lang.Thread.State.WAITING``.


```java
import java.lang.reflect.Field;
import java.util.List;

public class FieldSpy<T> {
    public boolean[][] b = {{ false, false }, { true, true } };
    public String name  = "Alice";
    public List<Integer> list;
    public T val;

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Field f = c.getField(args[1]);
	    System.out.format("Type: %s%n", f.getType());
	    System.out.format("GenericType: %s%n", f.getGenericType());

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}
```

There are several modifiers that may be part of a field declaration:

+ Access modifiers: public, protected, and private
+ Field-specific modifiers governing runtime behavior: transient and volatile
+ Modifier restricting to one instance: static
+ Modifier prohibiting value modification: final
+ Annotations

The FieldModifierSpy example illustrates how to search for fields with a given modifier. It also determines whether the located field is synthetic (compiler-generated) or is an enum constant by invoking ``Field.isSynthetic()`` and ``Field.isEnumCostant()`` respectively.


```java
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
import static java.lang.System.out;

enum Spy { BLACK , WHITE }

public class FieldModifierSpy {
    volatile int share;
    int instance;
    class Inner {}

    public static void main(String... args) {
        try {
            Class<?> c = Class.forName(args[0]);
            int searchMods = 0x0;
            for (int i = 1; i < args.length; i++) {
                searchMods |= modifierFromString(args[i]);
            }

            Field[] flds = c.getDeclaredFields();
            out.format("Fields in Class '%s' containing modifiers:  %s%n",
                    c.getName(),
                    Modifier.toString(searchMods));
            boolean found = false;
            for (Field f : flds) {
                int foundMods = f.getModifiers();
                // Require all of the requested modifiers to be present
                if ((foundMods & searchMods) == searchMods) {
                    out.format("%-8s [ synthetic=%-5b enum_constant=%-5b ]%n",
                            f.getName(), f.isSynthetic(),
                            f.isEnumConstant());
                    found = true;
                }
            }

            if (!found) {
                out.format("No matching fields%n");
            }

            // production code should handle this exception more gracefully
        } catch (ClassNotFoundException x) {
            x.printStackTrace();
        }
    }

    private static int modifierFromString(String s) {
        int m = 0x0;
        if ("public".equals(s))           m |= Modifier.PUBLIC;
        else if ("protected".equals(s))   m |= Modifier.PROTECTED;
        else if ("private".equals(s))     m |= Modifier.PRIVATE;
        else if ("static".equals(s))      m |= Modifier.STATIC;
        else if ("final".equals(s))       m |= Modifier.FINAL;
        else if ("transient".equals(s))   m |= Modifier.TRANSIENT;
        else if ("volatile".equals(s))    m |= Modifier.VOLATILE;
        return m;
    }
}
```

```java
import java.lang.reflect.Field;
import java.util.Arrays;
import static java.lang.System.out;

enum Tweedle { DEE, DUM }

public class Book {
    public long chapters = 0;
    public String[] characters = { "Alice", "White Rabbit" };
    public Tweedle twin = Tweedle.DEE;

    public static void main(String... args) {
	Book book = new Book();
	String fmt = "%6S:  %-12s = %s%n";

	try {
	    Class<?> c = book.getClass();

	    Field chap = c.getDeclaredField("chapters");
	    out.format(fmt, "before", "chapters", book.chapters);
  	    chap.setLong(book, 12);
	    out.format(fmt, "after", "chapters", chap.getLong(book));

	    Field chars = c.getDeclaredField("characters");
	    out.format(fmt, "before", "characters",
		       Arrays.asList(book.characters));
	    String[] newChars = { "Queen", "King" };
	    chars.set(book, newChars);
	    out.format(fmt, "after", "characters",
		       Arrays.asList(book.characters));

	    Field t = c.getDeclaredField("twin");
	    out.format(fmt, "before", "twin", book.twin);
	    t.set(book, Tweedle.DUM);
	    out.format(fmt, "after", "twin", t.get(book));

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}
```

Setting a field's value via reflection has a certain amount of performance overhead because various operations must occur such as validating access permissions. From the runtime's point of view, the effects are the same, and the operation is as atomic as if the value was changed in the class code directly.Use of reflection can cause some runtime optimizations to be lost. For example, the following code is highly likely be optimized by a Java virtual machine:

```java
int x = 1;
x = 2;
x = 3;
```

Equivalent code using ``Field.set*()`` may not.

When using reflection to set or get a field, the compiler does not have an opportunity to perform boxing. It can only convert types that are related as described by the specification for ``Class.isAssignableFrom()``. The example is expected to fail because ``isAssignableFrom()`` will return false in this test which can be used programmatically to verify whether a particular conversion is possible:``Integer.class.isAssignableFrom(int.class) == false``,Similarly, automatic conversion from primitive to reference type is also impossible in reflection.``int.class.isAssignableFrom(Integer.class) == false``

The ``Class.getField()`` and ``Class.getFields()`` methods return the ``public`` member field(s) of the class, enum, or interface represented by the Class object. To retrieve all fields declared (but not inherited) in the Class, use the ``Class.getDeclaredFields()`` method.

An access restriction exists which prevents final fields from being set after initialization of the class. However, Field is declared to extend ``AccessibleObject`` which provides the ability to suppress this check.If ``AccessibleObject.setAccessible()`` succeeds, then subsequent operations on this field value will not fail do to this problem. This may have unexpected side-effects; for example, sometimes the original value will continue to be used by some sections of the application even though the value has been modified. ``AccessibleObject.setAccessible()`` will only succeed if the operation is allowed by the security context.


### Methods

Methods have return values, parameters, and may throw exceptions. The ``java.lang.reflect.Method`` class provides methods for obtained the type information for the parameters and return value. 

A method contains executable code which may be invoked. Methods are inherited and in non-reflective code behaviors such as overloading, overriding, and hiding are enforced by the compiler. In contrast, reflective code makes it possible for method selection to be restricted to a specific class without considering its superclasses. Superclass methods may be accessed but it is possible to determine their declaring class; this is impossible to discover programmatically without reflection and is the source of many subtle bugs.

A method declaration includes the name, modifiers, parameters, return type, and list of throwable exceptions. The ``java.lang.reflect.Method`` class provides a way to obtain this information.

```java
import java.lang.reflect.Method;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class MethodSpy {
    private static final String  fmt = "%24s: %s%n";

    // for the morbidly curious
    <E extends RuntimeException> void genericThrow() throws E {}

    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		if (!m.getName().equals(args[1])) {
		    continue;
		}
		out.format("%s%n", m.toGenericString());

		out.format(fmt, "ReturnType", m.getReturnType());
		out.format(fmt, "GenericReturnType", m.getGenericReturnType());

		Class<?>[] pType  = m.getParameterTypes();
		Type[] gpType = m.getGenericParameterTypes();
		for (int i = 0; i < pType.length; i++) {
		    out.format(fmt,"ParameterType", pType[i]);
		    out.format(fmt,"GenericParameterType", gpType[i]);
		}

		Class<?>[] xType  = m.getExceptionTypes();
		Type[] gxType = m.getGenericExceptionTypes();
		for (int i = 0; i < xType.length; i++) {
		    out.format(fmt,"ExceptionType", xType[i]);
		    out.format(fmt,"GenericExceptionType", gxType[i]);
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}
```


You can obtain the names of the formal parameters of any method or constructor with the method ``java.lang.reflect.Executable.getParameters``. (The classes ``Method`` and ``Constructor`` extend the class ``Executable`` and therefore inherit the method ``Executable.getParameters``.) However, .class files do not store formal parameter names by default. This is because many tools that produce and consume class files may not expect the larger static and dynamic footprint of .class files that contain parameter names. In particular, these tools would have to handle larger .class files, and the Java Virtual Machine (JVM) would use more memory. In addition, some parameter names, such as secret or password, may expose information about security-sensitive methods.To store formal parameter names in a particular .class file, and thus enable the Reflection API to retrieve formal parameter names, compile the source file with the ``-parameters`` option to the ``javac`` compiler.

Certain constructs are implicitly declared in the source code if they have not been written explicitly. For example, the ExampleMethods example does not contain a constructor. A default constructor is implicitly declared for it. 

The Java compiler creates a formal parameter for the constructor of an inner class to enable the compiler to pass a reference (representing the immediately enclosing instance) from the creation expression to the member class's constructor.

``Method`` implements ``java.lang.reflect.AnnotatedElement``. Thus any runtime annotations with ``java.lang.annotation.RetentionPolicy.RUNTIME`` may be retrieved.

Reflection provides a means for invoking methods on a class. Typically, this would only be necessary if it is not possible to cast an instance of the class to the desired type in non-reflective code. Methods are invoked with ``java.lang.reflect.Method.invoke()``. The first argument is the object instance on which this particular method is to be invoked. (If the method is static, the first argument should be null.) Subsequent arguments are the method's parameters. If the underlying method throws an exception, it will be wrapped by an ``java.lang.reflect.InvocationTargetException``. The method's original exception may be retrieved using the exception chaining mechanism's ``InvocationTargetException.getCause()`` method.

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Type;
import java.util.Locale;
import static java.lang.System.out;
import static java.lang.System.err;

public class Deet<T> {
    private boolean testDeet(Locale l) {
	// getISO3Language() may throw a MissingResourceException
	out.format("Locale = %s, ISO Language Code = %s%n", l.getDisplayName(), l.getISO3Language());
	return true;
    }

    private int testFoo(Locale l) { return 0; }
    private boolean testBar() { return true; }

    public static void main(String... args) {
	if (args.length != 4) {
	    err.format("Usage: java Deet <classname> <langauge> <country> <variant>%n");
	    return;
	}

	try {
	    Class<?> c = Class.forName(args[0]);
	    Object t = c.newInstance();

	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		String mname = m.getName();
		if (!mname.startsWith("test")
		    || (m.getGenericReturnType() != boolean.class)) {
		    continue;
		}
 		Type[] pType = m.getGenericParameterTypes();
 		if ((pType.length != 1)
		    || Locale.class.isAssignableFrom(pType[0].getClass())) {
 		    continue;
 		}

		out.format("invoking %s()%n", mname);
		try {
		    m.setAccessible(true);
		    Object o = m.invoke(t, new Locale(args[1], args[2], args[3]));
		    out.format("%s() returned %b%n", mname, (Boolean) o);

		// Handle any exceptions thrown by method to be invoked.
		} catch (InvocationTargetException x) {
		    Throwable cause = x.getCause();
		    err.format("invocation of %s failed: %s%n",
			       mname, cause.getMessage());
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}
```

### Constructors

The Reflection APIs for constructors are defined in java.lang.reflect.Constructor and are similar to those for methods, with two major exceptions: first, constructors have no return values; second, the invocation of a constructor creates a new instance of an object for a given class.

A constructor is used in the creation of an object that is an instance of a class. Typically it performs operations required to initialize the class before methods are invoked or fields are accessed. Constructors are never inherited.

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorSift {
    public static void main(String... args) {
	try {
	    Class<?> cArg = Class.forName(args[1]);

	    Class<?> c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		Class<?>[] pType  = ctor.getParameterTypes();
		for (int i = 0; i < pType.length; i++) {
		    if (pType[i].equals(cArg)) {
			out.format("%s%n", ctor.toGenericString());

			Type[] gpType = ctor.getGenericParameterTypes();
			for (int j = 0; j < gpType.length; j++) {
			    char ch = (pType[j].equals(cArg) ? '*' : ' ');
			    out.format("%7c%s[%d]: %s%n", ch,
				       "GenericParameterType", j, gpType[j]);
			}
			break;
		    }
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}
```

Because of the role of constructors in the language, fewer modifiers are meaningful than for methods:

+ Access modifiers: public, protected, and private
+ Annotations


```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorAccess {
    public static void main(String... args) {
	try {
	    Class<?> c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		int searchMod = modifierFromString(args[1]);
		int mods = accessModifiers(ctor.getModifiers());
		if (searchMod == mods) {
		    out.format("%s%n", ctor.toGenericString());
		    out.format("  [ synthetic=%-5b var_args=%-5b ]%n",
			       ctor.isSynthetic(), ctor.isVarArgs());
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static int accessModifiers(int m) {
	return m & (Modifier.PUBLIC | Modifier.PRIVATE | Modifier.PROTECTED);
    }

    private static int modifierFromString(String s) {
	if ("public".equals(s))               return Modifier.PUBLIC;
	else if ("protected".equals(s))       return Modifier.PROTECTED;
	else if ("private".equals(s))         return Modifier.PRIVATE;
	else if ("package-private".equals(s)) return 0;
	else return -1;
    }
}
```

``Constructors`` implement ``java.lang.reflect.AnnotatedElement``, which provides methods to retrieve runtime annotations with ``java.lang.annotation.RetentionPolicy.RUNTIME``.

There are two reflective methods for creating instances of classes: ``java.lang.reflect.Constructor.newInstance()`` and ``Class.newInstance()``. The former is preferred and is thus used in these examples because:

+ ``Class.newInstance()`` can only invoke the zero-argument constructor, while ``Constructor.newInstance()`` may invoke any constructor, regardless of the number of parameters.
+ ``Class.newInstance()`` throws any exception thrown by the constructor, regardless of whether it is checked or unchecked. ``Constructor.newInstance()`` always wraps the thrown exception with an ``InvocationTargetException``.
+ ``Class.newInstance()`` requires that the constructor be visible; ``Constructor.newInstance()`` may invoke private constructors under certain circumstances.

```java
import java.io.Console;
import java.nio.charset.Charset;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

public class ConsoleCharset {
    public static void main(String... args) {
	Constructor[] ctors = Console.class.getDeclaredConstructors();
	Constructor ctor = null;
	for (int i = 0; i < ctors.length; i++) {
	    ctor = ctors[i];
	    if (ctor.getGenericParameterTypes().length == 0)
		break;
	}

	try {
	    ctor.setAccessible(true);
 	    Console c = (Console)ctor.newInstance();
	    Field f = c.getClass().getDeclaredField("cs");
	    f.setAccessible(true);
	    out.format("Console charset         :  %s%n", f.get(c));
	    out.format("Charset.defaultCharset():  %s%n",
		       Charset.defaultCharset());

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
 	} catch (InvocationTargetException x) {
 	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}
```

``Class.newInstance()`` will only succeed if the constructor is has zero arguments and is already accessible. Otherwise, it is necessary to use ``Constructor.newInstance()``. 

## Arrays and Enumerated Types

From the Java virtual machine's perspective, ``arrays`` and ``enumerated types (or enums)`` are just classes. Many of the methods in ``Class`` may be used on them. Reflection provides a few specific APIs for arrays and enums.

### Arrays

Arrays have a component type and a length (which is not part of the type). Arrays may be maniuplated either in their entirety or component by component. Reflection provides the ``java.lang.reflect.Array`` class for the latter purpose.

An array is an object of reference type which contains a fixed number of components of the same type; the length of an array is immutable. Creating an instance of an array requires knowledge of the length and component type. Each component may be a primitive type (e.g. byte, int, or double), a reference type (e.g. String, Object, or java.nio.CharBuffer), or an array. Multi-dimensional arrays are really just arrays which contain components of array type.

Arrays are implemented in the Java virtual machine. The only methods on arrays are those inherited from Object. The length of an array is not part of its type; arrays have a length field which is accessible via ``java.lang.reflect.Array.getLength()``.

```java
import java.lang.reflect.Field;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ArrayFind {
    public static void main(String... args) {
	boolean found = false;
 	try {
	    Class<?> cls = Class.forName(args[0]);
	    Field[] flds = cls.getDeclaredFields();
	    for (Field f : flds) {
 		Class<?> c = f.getType();
		if (c.isArray()) {
		    found = true;
		    out.format("%s%n"
                               + "           Field: %s%n"
			       + "            Type: %s%n"
			       + "  Component Type: %s%n",
			       f, f.getName(), c, c.getComponentType());
		}
	    }
	    if (!found) {
		out.format("No array fields%n");
	    }

        // production code should handle this exception more gracefully
 	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}
```

The number of '[' characters at the beginning of the type name indicates the number of dimensions (i.e. depth of nesting) of the array.

Just as in non-reflective code, reflection supports the ability to dynamically create arrays of arbitrary type and dimensions via ``java.lang.reflect.Array.newInstance()``. Consider ArrayCreator, a basic interpreter capable of dynamically creating arrays. The syntax that will be parsed is as follows:``fully_qualified_class_name variable_name[] = { val1, val2, val3, ... }``.Assume that the ``fully_qualified_class_name`` represents a class that has a constructor with a single String argument. The dimensions of the array are determined by the number of values provided. The following example will construct an instance of an array of ``fully_qualified_class_name`` and populate its values with instances given by ``val1, val2``, etc. (This example assumes familiarity with ``Class.getConstructor()`` and ``java.lang.reflect.Constructor.newInstance()``. 

```java
import java.lang.reflect.Array;
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import java.util.Arrays;
import static java.lang.System.out;

public class ArrayCreator {
    private static String s = "java.math.BigInteger bi[] = { 123, 234, 345 }";
    private static Pattern p = Pattern.compile("^\\s*(\\S+)\\s*\\w+\\[\\].*\\{\\s*([^}]+)\\s*\\}");

    public static void main(String... args) {
        Matcher m = p.matcher(s);

        if (m.find()) {
            String cName = m.group(1);
            String[] cVals = m.group(2).split("[\\s,]+");
            int n = cVals.length;

            try {
                Class<?> c = Class.forName(cName);
                Object o = Array.newInstance(c, n);
                for (int i = 0; i < n; i++) {
                    String v = cVals[i];
                    Constructor ctor = c.getConstructor(String.class);
                    Object val = ctor.newInstance(v);
                    Array.set(o, i, val);
                }

                Object[] oo = (Object[])o;
                out.format("%s[] = %s%n", cName, Arrays.toString(oo));

            // production code should handle these exceptions more gracefully
            } catch (ClassNotFoundException x) {
                x.printStackTrace();
            } catch (NoSuchMethodException x) {
                x.printStackTrace();
            } catch (IllegalAccessException x) {
                x.printStackTrace();
            } catch (InstantiationException x) {
                x.printStackTrace();
            } catch (InvocationTargetException x) {
                x.printStackTrace();
            }
        }
    }
}
$ java ArrayCreator
java.math.BigInteger [] = [123, 234, 345]
```

Just as in non-reflective code, an array field may be set or retrieved in its entirety or component by component. To set the entire array at once, use ``java.lang.reflect.Field.set(Object obj, Object value)``. To retrieve the entire array, use ``Field.get(Object)``. Individual components can be set or retrieved using methods in ``java.lang.reflect.Array``.

The components of arrays of reference types (including arrays of arrays) are set and retrieved using ``Array.set(Object array, int index, int value)`` and ``Array.get(Object array, int index)``.


```java
import java.io.BufferedReader;
import java.io.CharArrayReader;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.lang.reflect.Field;
import java.util.Arrays;
import static java.lang.System.out;

public class GrowBufferedReader {
    private static final int srcBufSize = 10 * 1024;
    private static char[] src = new char[srcBufSize];
    static {
	src[srcBufSize - 1] = 'x';
    }
    private static CharArrayReader car = new CharArrayReader(src);

    public static void main(String... args) {
	try {
	    BufferedReader br = new BufferedReader(car);

	    Class<?> c = br.getClass();
	    Field f = c.getDeclaredField("cb");

	    // cb is a private field
	    f.setAccessible(true);
	    char[] cbVal = char[].class.cast(f.get(br));

	    char[] newVal = Arrays.copyOf(cbVal, cbVal.length * 2);
	    if (args.length > 0 && args[0].equals("grow"))
		f.set(br, newVal);

	    for (int i = 0; i < srcBufSize; i++)
		br.read();

	    // see if the new backing array is being used
	    if (newVal[srcBufSize - 1] == src[srcBufSize - 1])
		out.format("Using new backing array, size=%d%n", newVal.length);
	    else
		out.format("Using original backing array, size=%d%n", cbVal.length);

        // production code should handle these exceptions more gracefully
	} catch (FileNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (IOException x) {
	    x.printStackTrace();
	}
    }
}
```

```java
import java.lang.reflect.Array;
import static java.lang.System.out;

public class CreateMatrix {
    public static void main(String... args) {
        Object matrix = Array.newInstance(int.class, 2, 2);
        Object row0 = Array.get(matrix, 0);
        Object row1 = Array.get(matrix, 1);

        Array.setInt(row0, 0, 1);
        Array.setInt(row0, 1, 2);
        Array.setInt(row1, 0, 3);
        Array.setInt(row1, 1, 4);

        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                out.format("matrix[%d][%d] = %d%n", i, j, ((int[][])matrix)[i][j]);
    }
}
```








### Enumerated Types

Enums are treated very much like ordinary classes in reflection code. ``Class.isEnum()`` tells whether a Class represents and enum. ``Class.getEnumConstants()`` retrieves the enum constants defined in an enum. ``java.lang.reflect.Field.isEnumConstant()`` indicates whether a field is an enumerated type.
