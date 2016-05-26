---
layout: post
category: 技术
tags: [Java]
title: Java注解（Annotation）
header: no
---

之前对注解的认识，只是局限在`@Override` `@Deprecated` `@SuppressWarnings`，好处也只是知道使用注解可以有更加干净易读的代码、可以在编译期进行类型检查，其他的知之甚少。但随着目前新技术的蓬勃发展，各种新技术、新框架涌现了出来，其中好多会用到注解，目前知道的有`Dragger2` `EventBus3` `Retrofit` `ActiveAndroid`等等。如果不深入了解注解的相关知识，可能在学习这些新东西时徒增一些困惑。所以接下来，我会由浅入深地带你走进注解的殿堂，一起学习，一起探索。

## 基本用法

在下面的例子中，使用`@Test`对`testExecute()`方法进行注解。该注解本身并不做任何事情，我们可以创建一个通过反射机制来运行`testExecute()`方法的工具。

{% highlight java %}
public class Testable {

    public void execute() {
        System.out.println("Executing...");
    }

    @Test
    void testExecute() {
        execute();
    }
}
{% endhighlight %}

被注解的方法与其他方法并没有区别。在这个例子中，注解`@Test`可以与任何修饰符共同作用于方法，例如`public`、`static`或`void`。从语法的角度看，注解和修饰符的使用方式几乎一摸一样。

### 定义注解

可以看到，注解的定义看起来很像接口的定义。事实上，与其他任何Java接口一样，注解也将会编译成class文件。

{% highlight java %}
import java.lang.annotation.*;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {
}
{% endhighlight %}

定义注解时，会需要一些`元注解`（meta-annotation），如`@Target`和`@Retention`。`@Target`用来定义你的注解将应用于什么地方（例如是一个方法或者一个域）。`@Retention`用来定义注解的可用级别，在源代码中（SOURCE）、类文件中（CLASS）或者运行时（RUNTIME）。

在注解中，一般都会包含一些元素以表示某些值。当分析处理注解时，程序或工具可以利用这些值。注解的元素看起来就像接口的方法，唯一的区别是你可以为其指定默认值。
没有元素的注解称为`标记注解`（marker annotation），例如上例中的`@Test`。

下面看看有元素的注解：

{% highlight java %}
import java.lang.annotation.*;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UseCase {
    int id();
    String description() default "no description";
}
{% endhighlight %}

注意，id和description类似方法定义。description元素有一个default值，如果在注解某个方法时没有给出description的值，则该注解的处理器就会使用此元素的默认值。在下面的类中，有三个方法被注解：

{% highlight java %}
import java.util.List;

public class PasswordUtils {
    @UseCase(id = 47, description = "Passwords must contain at least one numeric")
    public boolean validatePassword(String password) {
        return password.matches("\\w*\\d\\w*");
    }

    @UseCase(id = 48)
    public String encryptPassword(String password) {
        return new StringBuilder(password).reverse().toString();
    }

    @UseCase(id = 49, description = "New passwords can't equal previously used ones")
    public boolean checkForNewPassword(List<String> prevPasswords, String password) {
        return !prevPasswords.contains(password);
    }
}
{% endhighlight %}

注解的元素在使用时表现为key-value键值对的形式。在`encryptPassword()`方法的注解中，并没有给出description元素的值，因此，在注解处理器分析处理这个类时会使用该元素的默认值。

### 元注解

Java目前只内置了三种标准注解，以及四中元注解。元注解专职负责注解其他的注解：

> ---
> `@Target` 表示该注解可以用于什么地方。
>
> 可能的ElementType参数包括：<br>
> `CONSTRUCTOR` 构造器的声明<br>
> `FIELD` 域声明（包括enum实例）<br>
> `LOCAL_VARIABLE` 局部变量声明<br>
> `METHOD` 方法声明<br>
> `PACKAGE` 包声明<br>
> `PARAMETER` 参数声明<br>
> `TYPE` 类、接口（包括注解类型）或enum声明
>
> ---
>
> `@Retention` 表示需要在什么级别保存该注解信息。
>
> `SOURCE` 注解将被编译器丢弃<br>
> `CLASS` 注解在class文件中可用，但会被VM丢弃。<br>
> `RUNTIME` VM将在运行期也保留注解，因此可以通过反射机制读取注解的信息。<br>
>
> ---
>
> `@Documented` 将此注解包含在Javadoc中
>
> ---
>
> `@Inherited` 允许子类继承父类中的注解
>
> ---

## 编写注解处理器

如果没有用来读取注解的工具，那么注解也不会比注释更有用。使用注解的过程中，很重要的一个部分就是创建与使用注解处理器。Java SE5扩展了反射机制的API，已帮助我们构造这类工具。同时，它还提供了一个外部工具apt帮助我们解析带有注解的Java源代码。

下面是一个非常简单的注解处理器，我们将用它来读取PasswordUtils类，并使用反射机制查找`@UseCase`标记。

{% highlight java %}
import java.lang.reflect.Method;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class UseCaseTracker {
    public static void trackUseCases(List<Integer> useCases, Class<?> cl) {
        for (Method m : cl.getDeclaredMethods()) {
            UseCase uc = m.getAnnotation(UseCase.class);
            if (uc != null) {
                System.out.println("Found Use Case: " + uc.id() + " " + uc.description());
                useCases.remove(new Integer(uc.id()));
            }
        }
        for (int i : useCases) {
            System.out.println("Warning: Missing use case " + i);
        }
    }

    public static void main(String[] args) {
        List<Integer> useCases = new ArrayList<>();
        Collections.addAll(useCases, 47, 48, 49, 50);
        trackUseCases(useCases, PasswordUtils.class);
    }
}
{% endhighlight %}

```
Output:
Found Use Case: 47 Passwords must contain at least one numeric
Found Use Case: 48 no description
Found Use Case: 49 New passwords can't equal previously used ones
Warning: Missing use case 50
```
