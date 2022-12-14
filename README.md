# cpp-questions

## for each + remove if

You have a vector with timestamped data. You want to execute a task for all data that has a timestamp in the past compared to the current time. In addition processed elements should be removed from the vector. Is there a better way to do this than the following loop, that modifies the loop iterator?

```C++
    int current_time = 4;

    auto it = work.begin();
    while (it != work.end()) {
        if (it->timestamp > current_time) {
            do_work(it->id, it->parameters);
            it = work.erase(it);
        }
        else {
            it++;
        }
    }
```

Minimal working example ([compiler explorer](https://godbolt.org/z/3Th3df4GY)):
```C++
#include <iostream>
#include <vector>
#include <string>

struct Data {
    int id;
    int timestamp;
    std::string parameters;
};

void do_work(int id, const std::string& params) {
    std::cout << "Working on: " << id << ": " << params << "\n";
}

int main()
{
    std::vector<Data> work = {
        {1, 4, "one"},
        {2, 5, "two"},
        {3, 2, "three"},
        {4, 9, "four"},
    };

    int current_time = 4;

    auto it = work.begin();
    while (it != work.end()) {
        if (it->timestamp > current_time) {
            do_work(it->id, it->parameters);
            it = work.erase(it);
        }
        else {
            it++;
        }
    }

    std::cout << "\nAfter loop:\n";

    for (auto x : work) {
        std::cout << x.id << ": " << x.parameters << "\n";
    }
}
```

**answer**
The C++ Primer book [1] has some advice on this on page 354, section "Writing loops that change a container". It points out that the operations `insert` and `erase` return an iterator that should be used to refresh the loop iterator accordingly. In addition the next section reminds us to update the `end()` iterator. The code above follows all this advice.

## Real-time programming and threads

[This talk](https://www.youtube.com/watch?v=Tof5pRedskI) mentions that of all the related primitives, only `std::atomic` is real-time safe. Why is this and how to handle this?
