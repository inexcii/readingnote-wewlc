# Chapter 13. I Need to Make a Change, but I Don't Know What Tests to Write

## Points:

* In nearly every legacy system, what the system does is more important than what it is supposed to do(such as described in old requirements documents).
* Write characterization tests.
    * A test that characterizes the actual behavior of a piece of code.
* Algorithm for writing characterization tests:
    1. Use a piece of code in a test harness.
    1. Write an assertion that you know will fail.
    1. Let the failure tell you what the behavior is.
    1. Change the test so that it expects the behavior that the code produces.
    1. Repeat.
* Sample:
```
      // 1. Use a piece of code in a test harness.
      // 2. Write an assertion that you know will fail.
      void testGenerator() {
        PageGenerator generator = new PageGenerator();
        assertEquals("fred", generator.generate());
      }
```
```
      // 3. Let the failure tell you what the behavior is.
      junit.framework.ComparisonFailure: expected:<fred> but was:<>
```
```
      // 4. Change the test so that it expects the behavior that the code produces.
      void testGenerator() {
        PageGenerator generator = new PageGenerator();
        assertEquals("", generator.generate());
      }
```

* The Method Use Rule
    1. Before you use a method in a legacy system, check to see if there are tests for it. If there aren't, write them.

* For Classes:
    * Figure out what the class does at a high level first.
    * Then write simplest test and then let our curiosity guide us from there.

* Heuristics:
    1. Introducing a sensing variable.
    1. List of the things that can go wrong, then test to trigger.
    1. Test extreme values of inputs.
    1. Write test to verify invariants.

* Tips for writing tests of characterizing classes:
    1. Characterizing tests are like any documentation, you have to think about what will be important to the reader.
    1. What's the most important thing to know when it's the first time to use a class.
    1. Put the tests in a good order that makes it easier for people to learn.
    1. Start with some easy cases that show the main intent of class. Then move into highlights.

* When You Find Bugs
    * If the system has never been deployed: you should fix it.
    * If the system has been deployed, you need to examine the possibility that someone is depending on that behavior, even though you see it as a bug.
    * When behavior is clearly in error, it should be fixed.

* Example：
```
  public class FuelShare
  {
    private long cost = 0;
    private double corpBase = 12.0;
    private ZonedHawthorneLease lease;
    ...
    public void addReading(int gallons, Date readingDate) {
      if (lease.isMonthly()) {
        if (gallons < Lease.CORP_MIN)
            cost += corpBase;
        elae
            cost += 1.2 * priceForGallons(gallons);
      }
      ...
      lease.postReading(readingDate, gallons);
    }
    ...
  }
```

```
  public class FuelShare
  {
    public void addReading(int gallons, Date readingDate) {
      cost += lease.computeValue(gallons, priceForGallons(gallons));
      ...
      lease.postReading(readingDate, gallons);
    }
    ...
  }

  public class ZonedHawthorneLease extends Lease
  {
    public long computeValue(int gallons, long totalPrice) {
      long cost = 0;
      if (lease.isMonthly()) {
        if (gallons < Lease.CORP_MIN)
            cost += corpBase;
        else
            cost += 1.2 * totalPrice;
      }
      return cost;
    }
    ...
  }
```

* Test Code:

```
  public void testValueForGallonsMoreThanCorpMin() {
    StandardLease lease = new StandardLease(Lease.MONTHLY);
    FuelShare share = new FuelShare(lease);

    share.addReading(FuelShare.CORP_MIN + 1, new Date());
    assertEquals(12, share.getCost());
  }
```

* Issue: long -> double
```
  public class FuelShare
  {
    //private long cost = 0;
    private double cost = 0.0;
    ...
  }
```

