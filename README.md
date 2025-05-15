# How to create the default or empty of any type in C#
## 1. Empty Array<T>
```
// Efficient: returns a shared empty array instance.
// Available since .NET Framework 4.6 and .NET Core 1.0+.
T[] emptyArray = Array.Empty<T>();

// Examples:
int[] emptyInts = Array.Empty<int>();
string[] emptyStrings = Array.Empty<string>();
```

## 2. Empty Array<T?>
```
T?[] emptyArray = Array.Empty<T?>();

// Examples:
// Empty array of nullable integers
int?[] emptyNullableIntArray = new int?[0];

// Another way to create an empty array
int?[] anotherEmptyArray = Array.Empty<int?>();

Console.WriteLine(emptyNullableIntArray.Length); // Output: 0
Console.WriteLine(anotherEmptyArray.Length);     // Output: 0

// Checking for Emptiness:
int?[] nullableIntArray = GetNullableIntArray(); // Assume this method returns an array

if (nullableIntArray.Length == 0)
{
    Console.WriteLine("The nullable integer array is empty.");
}

// Using LINQ (requires System.Linq namespace)
if (!nullableIntArray.Any())
{
    Console.WriteLine("The nullable integer array is also empty (using LINQ).");
}
```

## 3. Default of Nullable Type (T?)
```
T? nullableDefault = default(T?);

// => default(T?) is null.

// Examples:
int? defaultInt = default(int?);            // null
DateTime? defaultDate = default(DateTime?); // null

// Or just assign null directly:
int? x = null;
```

## 4. Combined in Generics
If you want to handle both in a generic method:
```
public static class ObjectExtensions
{
    public static object GetEmptyOrDefault(Type type)
    {
        if (type.IsArray)
        {
            var elementType = type.GetElementType();
            return Array.CreateInstance(elementType, 0); // new T[0]
        }
    
        if (Nullable.GetUnderlyingType(type) != null)
        {
            return null; // nullable default
        }
    
        return type.IsValueType ? Activator.CreateInstance(type) : null;
    }
}
```
