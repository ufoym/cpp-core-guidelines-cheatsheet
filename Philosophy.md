# C++ Core Guidelines Philosophy
Bjarne Stroustrup & Herb Sutter
https://github.com/ufoym/cpp-core-guidelines-cheatsheet

## P.1: Express ideas directly in code
This is explicit about returning a **Month**, and explicit about not modifying the state of the **Date** object.
```c++
class Date {
public:
  Month month() const;
  // ...
};
```
This leaves the reader guessing and opens more possibilities for uncaught bugs.
```c++
class Date {
public:
  int month();
  // ...
};
```
---

This is bad, plus you should use **gsl::index**
```c++
void f(vector<string>& v)
{
  string val;
  cin >> val;
  // ...
  int index = -1;
  for (int i = 0; i < v.size(); ++i) {
    if (v[i] == val) {
      index = i;
      break;
    }
  }
  // ...
}
```

A much clearer expression of intent. A C++ programmer should know the basics of the standard library, and use it where appropriate.

```c++
void f(vector<string>& v)
{
  string val;
  cin >> val;
  // ...
  auto p = find(begin(v), end(v), val);
  // ...
}
```
---

Bad: what does s signify?

```c++
change_speed(double s);
// ...
change_speed(2.3);
```

Better: the meaning of s is specified
```c++
change_speed(Speed s);
// ...
change_speed(2.3); // error: no unit
change_speed(23_m / 10s); // meters per second
```

## P.2: Write in ISO standard C++

There are environments where extensions are necessary, e.g., to access system resources. In such cases, localize the use of necessary extensions and control their use with non-core Coding Guidelines. If possible, build interfaces that encapsulate the extensions so they can be turned off or compiled away on systems that do not support those extensions.

Extensions often do not have rigorously defined semantics. Even extensions that are common and implemented by multiple compilers might have slightly different behaviors and edge case behavior as a direct result of not having a rigorous standard definition. With sufficient use of any such extension, expected portability will be impacted.
