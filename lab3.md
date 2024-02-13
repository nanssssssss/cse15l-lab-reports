# Lab Report 3 # 

### Failure-Inducing JUnit Tests ###
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
  public void testReverseInPlace() {
    int[] arr = {1, 2, 3, 4, 5};
    ArrayExamples.reverseInPlace(arr);
    assertArrayEquals(new int[]{5, 4, 3, 2, 1}, arr);
  }

  @Test
  public void testReversed() {
    int[] arr = {1, 2, 3, 4, 5};
    assertArrayEquals(new int[]{5, 4, 3, 2, 1}, ArrayExamples.reversed(arr));
  }
}
```

### Non-Failure-Inducing JUnit Tests ###
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
  public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
  }

  @Test
  public void testReversed() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
}
```

### Symptom of ArrayExamples.java ###
![Image](symptom.png)

### Buggy ArrayExamples.java Code ###
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }

  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);
  }


}
```

### Debugged ArrayExamples.java Code ###
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    int[] tempArray = new int[arr.length];

    for(int i = 0; i < arr.length; i++) {
      tempArray[i] = arr[arr.length - i - 1];
    }

    for(int i = 0; i < arr.length; i++) {
      arr[i] = tempArray[i];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[arr.length - i - 1] = arr[i];
    }
    return newArray;
  }

  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);
  }

}
```
The original `reverseInPlace()` method mistakenly assigned the last element of the input array arr to the first element, causing
the loss of values for the initial elements due to overwriting. For instance, when arr[1] = 4, this statement overwrote it so that
when arr[4] was supposed to equal 1, arr[1] became 4, preventing the intended reversal. The revised code resolves this issue by 
storing the elements of the arr array in newArray, starting with the first element of arr and the last element of newArray. 
Subsequently, the elements of newArray are reassigned back to arr, ensuring the reversal is performed correctly.

In the original `reversed()` method, the input array arr was initially assigned to newArray in reverse order, but newArray remained 
empty, resulting in all elements of arr being set to 0. To address this bug, I reversed the assignment so that newArray now stores
the elements of the arr array in reverse order, resolving the issue of empty newArray and ensuring the correct reversal of elements.



