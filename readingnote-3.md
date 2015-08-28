# Chapter 3. Sensing and Separation

* Dependencies among classes can make it very difficult to get particular clusters of objects under test.
* One of the big problems that we confront in legacy code work is dependency.
* There are two reasons to break dependencies when we want to get tests in place:
    1. Sensing: When we can't access values our code computes.
    1. Separation: When we can't even get a piece of code into a test harness to run.

### Fakes Distilled
* In OO(Object-Oriented) languages, they are often implemented as simple classes.
* In non-OO languages, we can implement a fake by defining an alternative function, one which records values in some golbal data structure that we can access in tests.(Chapter 19, My Project is Not Object-Oriented. How Do I Make Safe Changes?)

### Mock Objects
* A more advanced type of fake which performs assertions internally.

## Example(for Sensing): A Sale System

* Background:
    * There are many ways to separate software which is introduced in the back of this book, but there is one dominant technique for sensing.
    * If we can put some other code in its place and test through it, we can write our tests.
    * In object orientation, these other pieces of code are often called fake objects.

* Goal:
    * Test(sense) scan function.
        * Input: Bar code
        * Output: Item name & price(in text)

```
public class Sale
{
  public void scan(String barcode) {
    ...
    // Code of displaying the item name and price is hidden deeply inside this method.
    ...
  }
}
```

* Step 1: Extract the code where the display is updated.

```
public class Sale
{
  private ArtR56Display artR56Display;

  public Sale(Art56Display display) {
    this.artR56Display = display;
  }

  public void scan(String barcode) { // Code of display has been refactored by artR56Display. }
}

public class ArtR56Display
{
  public showLine(String line) { // Code of display. }
}
```

* Step 2: Extract interface from the concrete class ArtR56Display

```
public class Sale
{
  private Display display;

  public Sale(Display display) {
    this.display = display;
  }

  public void scan(String barcode) { // Code of display has been refactored by display. }
}

public interface Display
{
 void showLine(String line);
}

public class ArtR56Display implements Display
{
  public showLine(String line) { // Code of display. }
}

```

* Why step 2?

```
public class FakeDisplay implements Display
{
  private String lastLine = "";

  public void showLine(String line) {
    lastLine = line;
  }

  public String getLastLine() {
    return lastLine;
  }
}
```

* Test code:

```
public class SaleTest extends TestCase
{
  public void testDisplayAnItem() {
    FakeDisplay display = new FakeDisplay();
    Sale sale = new Sale(display);

    sale.scan("1");
    assertEquals("Milk $3.99", display.getLastLine());
  }
}
```

### The Two Sides of a Fake Object
1. One side for the production code.
1. One side for the test code.

### Fake Objects Support Real Tests
* When we write tests, we have to divide and conquer.
* When we write tests for individual units, we end up with small, well-understood pieces.



# 分かってないこと

1. Step 1をやる時にテストは必要がないでしょうか？

# 困ってること
* 実践したいのに、実践コードがあまりない
