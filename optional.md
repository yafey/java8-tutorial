# Optional

我们在写程序时一定都遇到过 ```空指针异常```。它通常发生在你试图去引用一个没有被初始化，被初始化为空或者没有指向任何实例的对象的情况下。***空仅仅只是意味着没有值***，最有可能的是，罗马人是唯一没有遇到过空，从 1 开始计数 1,2,3...（没有0） 的人，或许，他们没有模拟市场上没有苹果的情况。[:-)]

“我叫它价值十亿美元的错误” —— [Sir C. A. R. Hoare](https://en.wikipedia.org/wiki/Tony_Hoare)，他在提出空引用这个概念时说。

在本文中，我将讨论 [java 8 新特性](https://howtodoinjava.com/category/java-8/) 中 [Optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) 的具体用法。为了阐明诸多概念之间的异同，本文将被划分为多个部分一一讲解。	

	讨论的要点

	1）什么是空类型？
	2）返回一个空值有什么问题？
	3）Java8中的Optional类提供了一种怎样的解决办法？
     a）创建一个Optional对象
	 b）有了一个值对它进行操作
     c）默认或者缺省的值和行为
     d）使用过滤器方法拒绝一些特殊值
    4）Optional内部是如何工作的？
    5）Optional是用来解决什么问题的？
    6）Optional不能解决什么问题？
	7）Optional如何使用呢？
	8）最后的结论

1）什么是Null类型？

在java中，我们通过引用类型去访问一个对象，当我们的的引用没有指向一个具体的对象，我们就会把这个引用置为Null，用于标识该引用没有值，对吧？

虽然null值的使用如此普遍，但是我们很少会去仔细地研究它。例如一个类的成员变量会被自动初始化为null，程序员通常会给没有初始值的引用类型置null，总体上说，null值主要用于我们不知道具体的或者没有具体指向的引用类型。

    补充，在java中，null是一个具体而又特殊的数据类型，因为它没有名字，所以我们声明一个类型为null的变量或者将一个类型
    强转为null；实际上，仅仅有一个能和null有关联的值（例如。。。）。请记住，null不像java中的其他类型，一个空引用能
    完全正确，安全地赋给其他的引用类型，而且不报任何错。


2）返回null值有什么问题？

通常来说，API的设计者会附上详细的文档，并说明在某种情况下，这个API会返回一个null值，现在问题是，因为一些原因，我们在调用API的时候可能不会去读API文档，然后就忘记了处理null值的情况，这在未来一定会是一个bug。

相信我，这是经常发生的，而且是空指针异常的主要原因之一，虽然并不是唯一的原因。因此，请牢记，在第一次调用API时看看它的文档说明（。。至少）[:-)]。

    现在我们知道了在大多数情况下，null值是一个问题，那么处理null值最好的方式是什么呢？

一个比较好的解决方案是在定义引用类型时总是去将它初始化为某个值，并且不要初始化为null。采用这种方式，你的程序永远不会抛出 ```空指针异常``` 。但是有些情况下，引用并没有一个默认的初始值，那么我们又该怎么去处理呢？

上述的答案都是正确的，而且在很多时候被使用。但是，Java 8 ```Optionals``` 无疑是最好的答案。

3）Java 8的Optionals提供了一种怎样的解决方案？

Optional提供了一种可以替代一个指向非空值但是可以为空T的方法。一个Optional类或许包含了一个指向非空变量的引用（这种情况下我们说这个引用是Present的），或者什么也没包含（这种情况下我们说这个引用是absent）。

请记住，永远不要说optional包含null
	

    Optional<Integer> canBeEmpty1 = Optional.of(5);
	canBeEmpty1.isPresent();                    // returns true
	canBeEmpty1.get();                          // returns 5
	 
	Optional<Integer> canBeEmpty2 = Optional.empty();
	canBeEmpty2.isPresent();                    // returns false

我们可以看到 **Optional作为保存单变量的容器，既可以保存值，也可以不保存值。**

必须要指出Optional类并不是为了简单地去替代每一个null引用。它的出现是为了**帮助设计更易于理解的API**，以便我们只需要读方法的声明，就能分辨是否是一个Optional变量。这迫使你去捕获Optional中的变量，然后处理它，同时也处理optional为空的情形。这才是解决因为空引用导致的 ```NullPointerException```。

下面是一些学习在编程中如何使用Optional的例子。

a) 创建一个Optional对象
 创建一个Optional对象有三种方式。

i)使用Optional.empty()方法

	Optional<Integer> possible = Optional.empty();

ii）使用Optional.of()方法创建一个带有非空默认值的对象，如果你赋值为null，则会马上抛出空指针异常。

	Optional<Integer> possible = Optional.of(5);

iii)使用Optional.ofNullable()创建一个带有null值的对象。如果值是null，则目标Optional对象将会是空(记得值是缺失，不是null)。

	Optional<Integer> possible = Optional.ofNullable(null);
	//or
	Optional<Integer> possible = Optional.ofNullable(5);

b)如果Optional的值是present的，来做一些操作吧
  得到Optional对象是第一步。现在我们先来检查一下它的内部是否保存了值，然后来使用它。

	Optional<Integer> possible = Optional.of(5);
	possible.ifPresent(System.out::println);

你可以像下面的代码一样重写你的代码。然而这并不是Optional类的最佳实践，因为这样做和手动检查是否为null没有任何区别，没有任何提升。

	if(possible.isPresent()){
    System.out.println(possible.get());
	}

如果Optional对象是空的，不会输出任何东西。

c)默认或者缺省的值和动作
一个典型的编程模式是如果操作的结果是null，那么最好返回一个默认的值。简要地说，你可以      但是如果使用Optional，你可以像下面那样写你的代码。

	//Assume this value has returned from a method
	Optional<Company> companyOptional = Optional.empty();
	 
	//Now check optional; if value is present then return it,
	//else create a new Company object and retur it
	Company company = companyOptional.orElse(new Company());
	 
	//OR you can throw an exception as well
	Company company = companyOptional.orElseThrow(IllegalStateException::new);

d)使用过滤方法拒绝某些特定的值

我们经常调用某个类中的方法去检查一些属性，例如，在下面的示例代码检查Company类是否有 “Finance”部门；如果有，则输出其值。

	Optional<Company> companyOptional = Optional.empty();
	companyOptional.filter(department -> "Finance".equals(department.getName())
                    .ifPresent(() -> System.out.println("Finance is present"));

这个过滤方法起了论断的作用。如果Optional对象中值是Present的，并且符合了这个论断，这个过滤方法返回这个值；否则，它返回一个空的Optional对象，你可能已经看出来了，当我们使用过滤方法时，其实和Stream的模式是相似的。

很好，这些代码看起来更接近我们提出的问题，在也没有冗余的检查是否为空的代码。

Wow，我们


4）Optional类内部是如何实现的呢？

如果你打开Optional类的源代码，你将会看到如下定义：

	/**
 	* If non-null, the value; if null, indicates no value is present
 	*/
	private final T value;

如果你定义一个空的Optional，就像下面这样，static关键字保证了只存在一个空的实例。

	/**
 	* Common instance for {@code empty()}.
 	*/
	private static final Optional<?> EMPTY = new Optional<>();

默认的没有参数的构造函数被定义成private的，所以除了上面的三种方式，其他的都不能创建一个实例。

当你创建一个Optional类的对象时，下面的函数会被调用，然后将确定的值传递给value属性。
	
	this.value = Objects.requireNonNull(value);

当你试图从Optional容器中获取值时，值如果存在就会被返回，否则抛出 ```NoSuchElementException``` 异常：

	public T get() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
	}

同样的，定义在Optional类中其他的函数也只操作value属性，点击 [这里](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/util/Optional.java) 查看Optional类的源代码。

5）Optional类的出现是为了解决什么问题？

Optional试图解决在java整个系统中出现空指针异常的次数，通过设计考虑了返回值可能为null的更具表达意义的API接口来实现。如果Optional类在java设计之初就已经存在了，那么时下的一些函数库和程序可能能更好地处理返回值为null的情况，减少出现空指针异常的次数，总体上减少一些bug。

通过使用Optional类，用户不得不去考虑一些异常情况，除了通过给null一个名字而带来的程序可读性的提高，最大的好处就是              如果你想让你的程序通过编译，你就不得不去考虑值为null的情况，


6）Optional不能解决什么问题？
  
Optional类并不是一种可以避免所有空指针的的机制。例如：一个方法或者构造函数中必须的参数就必须做一下检测。

当使用null时，Optional不会表现出值缺失的意思。所以当你调用一个方法时，为了正确地调用，请查看相关的文档，确保你知道Optional类中值缺失的意思。

• in the domain model layer (it’s not serializable)
• in DTOs (it’s not serializable)
• in input parameters of methods
• in constructor parameters

7）我们应该使用Optional吗？
 
当函数的返回值有可能是null时，我们应该尽可能地去使用它。

下面是从OpenJDK中摘抄的一段文字：
	The JSR-335 EG felt fairly strongly that Optional should not be on any more than needed to support the optional-return idiom only.
	Someone suggested maybe even renaming it to “OptionalReturn“.

这段文字明确地表示，当从具体的业务逻辑层，数据库访问层或者实体中返回值时应该使用Optional，如果确定Optional能起到作用的话。

8)结论

在这篇文章中，我们学习到了如何理解和使用java SE8中java.util包下Optional类。Optional存在的目的不是为了替代我们代码中简单的null引用，而是帮助我们设计更清晰明了的API，通过读方法的说明，使用者就能知道有Optional类，可能存在null的情况，从而恰当地去处理它。

这就是这个厉害特性的所有内容，在下面的评论区和我交流你的想法吧。

尽情享受学习的乐趣吧！!