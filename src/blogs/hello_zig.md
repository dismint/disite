---
layout: 'layouts/blog.html'
tags: blog
date: 2025-04-28
length: 1 minute
title: Printing in Zig

---

Much like me, you might one day find yourself with a sudden desire to print something in Zig. This can be a daunting task to think about, but I'll show you how to do it!

```zig
const std = @import("std");

pub fn main() {
    std.debug.print("Mom, I'm printing a string!", .{});
}
```

Whew, that wasn't so bad was it? As a bonus, I'm going to show you how to print numbers in hexadecimal!

```zig
const std = @import("std");

pub fn main() {
    const number = 6202003;
    std.debug.print("{x}", .{number}); // 5ea293
}
```
Okay thanks for your time! Bye!
