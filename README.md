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
## 2. Default of Nullable Type (T?)
```
T? nullableDefault = default(T?);

// => default(T?) is null.

// Examples:
int? defaultInt = default(int?);            // null
DateTime? defaultDate = default(DateTime?); // null

// Or just assign null directly:
int? x = null;
```
## 3. Combined in Generics
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
