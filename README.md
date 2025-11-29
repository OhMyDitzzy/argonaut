<p align="center">
  <img src="assets/argonaut_logo.png" alt="argonaut Logo" height="100">
</p>

<h1 align="center">argonaut</h1>

<p align="center">
  <b>Flexible and Powerful Command-Line Argument Parser for Zig</b>  
  <br>
  <i>Build beautiful CLI applications with ease.</i>
</p>

<p align="center">
  <a href="https://ziglang.org/download/">
    <img src="https://img.shields.io/badge/Zig-0.15.2-orange.svg?logo=zig&logoColor=white" alt="Zig Version">
  </a>
  <a href="https://github.com/OhMyDitzzy/argonaut/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/OhMyDitzzy/argonaut?color=blue" alt="License">
  </a>
  <a href="https://github.com/OhMyDitzzy/argonaut/stargazers">
    <img src="https://img.shields.io/github/stars/OhMyDitzzy/argonaut?style=social" alt="GitHub Stars">
  </a>
  <a href="https://github.com/OhMyDitzzy/argonaut/network/members">
    <img src="https://img.shields.io/github/forks/OhMyDitzzy/argonaut?style=social" alt="GitHub Forks">
  </a>
</p>

---

## Overview

A flexible and powerful command-line argument parser for Zig, inspired by the [Go argparse](https://github.com/akamensky/argparse) library. This library provides a clean and intuitive API for building complex CLI applications with support for subcommands, various argument types, validation, and more.

## Features

- **Multiple Argument Types**: Flags, counters, strings, integers, floats, files, and lists
- **Subcommands**: Build complex CLI tools with nested commands (like git)
- **Positional Arguments**: Support for positional arguments alongside named flags
- **Validation**: Custom validation functions for arguments
- **Default Values**: Set default values for optional arguments
- **Selector Arguments**: Restrict argument values to a predefined set
- **Automatic Help**: Built-in help generation with customizable formatting
- **Type Safety**: Fully type-safe argument handling
- **Modular Design**: Clean separation of concerns for easy maintenance

## Installation

Add the dependency in your build.zig.zon by running the following command:

```bash
zig fetch --save=argonaut git+https://github.com/OhMyDitzzy/argonaut
```

Then in your `build.zig`:

```zig
exe.root_module.addImport("argonaut", b.dependency("argonaut", .{ .target = target, .optimize = optimize }).module("argonaut"));
```

## Quick Start

```zig
const std = @import("std");
const argsparse = @import("argonaut");

pub fn main() !void {
    var gpa = std.heap.GeneralPurposeAllocator(.{}){};
    defer _ = gpa.deinit();
    const allocator = gpa.allocator();

    // Create a parser with a program name and description
    const parser = try argsparse.newParser(allocator, "myapp", "My awesome application");
    defer parser.deinit();

    // Define simple flags and values
    const verbose = try parser.flag("v", "verbose", "Enable verbose output");
    const name = try parser.string("n", "name", "User name");
    const count = try parser.int("c", "count", "Loop count");

    const args = try std.process.argsAlloc(allocator);
    defer std.process.argsFree(allocator, args);

    // You can handle parse errors and print usage on failure
    // try parser.parse(args) catch |err| {
    //    std.debug.print("Error: {}\n\n", .{err});
    //    const usage_text = try parser.usage(null);
    //    defer allocator.free(usage_text);
    //    std.debug.print("{s}", .{usage_text});
    //    std.process.exit(1);
    //};

    // Use the parsed values
    if (verbose.*) {
        std.debug.print("Verbose mode enabled\n", .{});
    }
    std.debug.print("Name: {s}, Count: {d}\n", .{ name.*, count.* });
}
```

## Examples

The library includes several examples:

- `examples/basic.zig` - Basic usage with various argument types
- `examples/subcommands.zig` - Git-like subcommand structure
- `examples/advanced.zig` - Advanced features including validation and lists

Build and run examples:

```bash
zig build example-basic -- --name John --age 25 -v
zig build example-subcommands -- commit -m "Initial commit" -a
zig build example-advanced -- -vvv -p 8080 -f file1.txt -f file2.txt input.txt
```
You can see all the available examples in the [examples](examples) folder.

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License
```
MIT License

Copyright (c) 2025 DitzDev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```