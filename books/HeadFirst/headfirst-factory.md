# 工厂模式测试
## 定义简单工厂
简单工厂其实不是一种设计模式，比较像一种编程习惯。但由于经常被使用，所以给它一个" Head First Patter 荣誉奖"。有些开发人员的确是把这个编程习惯误认为是“工厂模式” ,下次跟开发的朋友聊天无话可说的时候，这个应该是打破沉默的一个不错的选择。  

不要因为简单工厂不是一个“真正的设计模式” ， 就忽略的它的用法。

我们先看一下披萨店例子的类图：  

![factory](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory.png)

这就是简单工厂，下面要说的就是两个重量级的模式，它们都是工厂。

这里提醒一下：在设计模式中，所谓的”实现一个接口“并”不一定“表示”写一个类“ ，并继承这个接口。”实现一个接口“泛指”实现某个超类型（可以是类或者接口）的某个方法“。

## 工厂方法模式
所有工厂模式都用来封装对象的创建。工厂方法模式通过让子类决定该创建的对象是什么，来达到将兑现该创建的过程封装的目的。下面我们看一组类图来了解一下有哪些组成元素

- 创建者( Creator ) 类  
![factory2](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory2.png)

- 产品类  
![factory3](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory3.png)

我们已经看到，将一个 orderPizza() 方法和一个工厂方法联合起来，就可以成为一个框架。除此之外，工厂方法将生产只是封装进各个创建者，这样的做法，也可以被视为一个框架。

![factory4](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory4.png)

### 定义工厂方法模式
工厂方法模式：定义了一个创建对象的接口，但由于子类决定要实例化的类的哪个，工厂方法让类把实例化推迟到子类。

工厂方法模式能够封装具体类型的实例化。看下面的类图，抽象的 Creator 提供了一个创建对象的方法的接口，也成为”工厂方法“。在抽象的 Creator 中，任何其他实现的方法，都可能使用到这个工厂方法所制造出来的产品，但只有子类真正实现这个工厂方法并创建产品。

![factory5](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory5.png)

## 对象依赖
当你实例化一个对象时，就是在依赖它的具体类。之前说的那个依赖很高的比萨店例子，它由比萨店类来创建所有的比萨对象，而不是委托给工厂。

![factory6](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory6.png)

### 依赖倒置原则
设计原则：要依赖抽象，不要依赖具体类   
意思就是：不能让高层组件依赖底层组件，而且，不管高层或底层组件，”两者“都应该依赖于抽象。

所以在应用工厂方法之后，类图看起来就像这样：  
![factory7](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory7.png)

高层组件（也就是 PizzaStore ）和底层组件（也就是这些比萨）都依赖了 Pizza 抽象。想要遵循依赖倒置原则，工厂方法并非是唯一的技巧，但却是最有威力的技巧之一。

## 抽象工厂模式
抽线工厂模式：提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。

抽象工厂允许客户使用抽象的接口来创建一组相关的产品，而不需要知道实际产出的具体的产品是什么。这样客户就从具体的产品中被解耦。我们看一下下面的类图了解一下其中的关系。

![factory8](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory8.png)

我们再从 PizzaStore 的观点看下面这张相当复杂的类图：

![factory9](https://github.com/Lenzan/reading-notes/blob/master/books/HeadFirst/Texture/factory9.png)

这里细心的朋友已经观察到了，抽象工厂的每个方法实际上看起来都像是工厂方法（例如；createDough() , createSource() 等）。每个方法都被声明成抽象，而子类的方法覆盖这些方法来创建某些对象。这不正是工厂方法吗？是的，抽象工厂的方法经常以工厂方法的方式实现，抽象工厂的任务是定义一个负责创建一组产品的接口。这个接口内的每个方法都负责创建一个具体产品，同时我们利用实现抽象工厂的子类来提供这些具体的做法。所以，在抽象工厂中利用工厂方法实现生产方法是相当自然的做法。

### 代码示例（比萨店）

- 创建原料工厂

        public interface PizzaIngredientFactory()
        {
            public Dough CreateDoough();
            public Sauce CreateSauce();
            public Clam CreateClam();
            ...
        }

 - 创建纽约原料工厂

        public class NYPizzaIngredientFactory : PizzaIngredientFactory{
            public Dough createDough(){
                return new ThinCrustDough();
            }

            public Dough createSauce(){
                return new MarinaraSauce();
            }
    
             public Clam createClam(){
                return new FreshClams();
            }
        }

- 比萨抽象类

        public abstract class Pizza
        {
            string name;
            Dough dough;
            Sauce sauce;
    
            abstract void prepare();
    
            void Bake(){
                print("bake...");
            }
    
            void cut(){
                print("cut");
            }
    
            void Box()
            {
                print("box");
            }
    
            void SetName(string name){
                this.name = name;
            }
    
            string GetName(){
                return name;
            }
    
        }

- CheesePizza 类

        public class CheesePizza : Pizza{
            PizzaIngredientFactory ingredientFactory;
            public CheesePizza(PizzaIngredientFactory ingredientFactory){
                this.ingredientFactory = ingredientFactory;
            }
    
            void prepare(){
                print("prepare" + name)
                dough = ingredientFactory.CreateDough();
                sauce = ingredientFactory.CreateSauce();
            }
        }

- ClamPizza 类

        public class ClamPizza : Pizza{
            PizzaIngredientFactory ingredientFactory;
            public CheesePizza(PizzaIngredientFactory ingredientFactory){
                this.ingredientFactory = ingredientFactory;
            }
    
            void prepare(){
                print("prepare" + name)
                dough = ingredientFactory.CreateDough();
                sauce = ingredientFactory.CreateSauce();
                clam = ingredientFactory.CreateClam();
            }
        }

- 比萨店

        public class NYPizzaStore : PizzaStore
        {
            protected Pizza CreatePizza(string name){
                Pizza pizza = null;
                PizzaIngredientFactory ingredientFactory = new PizzaIngredientFactory();
    
                if(name.Equals("cheese")){
                    pizza = new CheesePizza(ingredientFactory);
                    pizza.SetName("New York Style Cheese Pizza");
                }
                else if(name.Equals("clam")){
                    pizza = new ClamPizza(ingredientFactory);
                    pizza.SetName("New York Style Clam Pizza");
                }
                ...
            }
        }
