# Contributing to argonaut

Thank you for your interest in contributing to argonaut! We appreciate your help in making this library better for everyone. This document provides guidelines and instructions for contributing.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Coding Guidelines](#coding-guidelines)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing](#testing)
- [Documentation](#documentation)
- [Questions?](#questions)

## 🤝 Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors. We expect everyone to:

- **Be Respectful**: Treat everyone with respect and kindness
- **Be Constructive**: Provide helpful feedback and be open to receiving it
- **Be Collaborative**: Work together to improve the project
- **Be Patient**: Remember that everyone has different skill levels and backgrounds

### Unacceptable Behavior

- Harassment, discrimination, or intimidation of any kind
- Trolling, insulting comments, or personal attacks
- Publishing others' private information without permission
- Any conduct that would be inappropriate in a professional setting

## 🚀 Getting Started

### Prerequisites

Before you begin, make sure you have:

- **Zig 0.15.2 or later** installed ([download here](https://ziglang.org/download/))
- **Git** for version control
- A **GitHub account** to submit pull requests
- Basic knowledge of **Zig programming language**

### First Time Contributors

If you're new to open source, welcome! Here are some good starting points:

1. **Browse Issues**: Look for issues labeled `good first issue` or `help wanted`
2. **Read the Code**: Familiarize yourself with the codebase structure
3. **Run Examples**: Try running the examples to understand how the library works
4. **Ask Questions**: Don't hesitate to ask for clarification on issues or in discussions

## 💡 How Can I Contribute?

### Reporting Bugs

If you find a bug, please create an issue with the following information:

**Bug Report Template:**

```markdown
**Description**
A clear and concise description of the bug.

**To Reproduce**
Steps to reproduce the behavior:
1. Create a parser with '...'
2. Add argument '...'
3. Parse args '...'
4. See error

**Expected Behavior**
What you expected to happen.

**Actual Behavior**
What actually happened.

**Environment**
- Zig Version: [e.g., 0.15.2]
- OS: [e.g., Ubuntu 22.04, Windows 11, macOS 14]
- argonaut Version: [e.g., commit hash or tag]

**Code Sample**
```zig
// Minimal code to reproduce the issue
const parser = try argparse.newParser(...);
// ...
```

**Additional Context**
Any other relevant information.
```

### Suggesting Enhancements

Have an idea for a new feature? We'd love to hear it! Create an issue with:

**Feature Request Template:**

```markdown
**Feature Description**
A clear description of the feature you'd like to see.

**Use Case**
Explain why this feature would be useful and what problem it solves.

**Proposed Solution**
How you think this feature could be implemented.

**Alternatives Considered**
Other approaches you've thought about.

**Additional Context**
Examples from other libraries, mockups, or any other relevant information.
```

### Improving Documentation

Documentation improvements are always welcome! You can help by:

- Fixing typos or grammatical errors
- Adding more examples
- Clarifying confusing sections
- Translating documentation (if applicable)
- Improving code comments

### Writing Code

You can contribute code by:

- Fixing bugs
- Implementing new features
- Improving performance
- Adding tests
- Refactoring existing code

## 🛠️ Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/argonaut.git
cd argonaut

# Add upstream remote
git remote add upstream https://github.com/OhMyDitzzy/argonaut.git
```

### 2. Create a Branch

```bash
# Create a new branch for your changes
git checkout -b feature/your-feature-name

# Or for bug fixes
git checkout -b fix/bug-description
```

### 3. Build and Test

```bash
# Build the library
zig build

# Run tests
zig build test

# Run examples
zig build example-basic
zig build example-subcommands
zig build example-advanced
```

### 4. Make Your Changes

- Write your code following the [Coding Guidelines](#coding-guidelines)
- Add tests for new functionality
- Update documentation as needed
- Ensure all tests pass

### 5. Commit Your Changes

```bash
git add .
git commit -m "feat: add new feature"
```

See [Commit Message Guidelines](#commit-message-guidelines) for commit message format.

## 📝 Coding Guidelines

### Code Style

We follow standard Zig conventions:

#### Naming Conventions

```zig
// Types: PascalCase
pub const Parser = struct { ... };
pub const ArgumentType = enum { ... };

// Functions: camelCase
pub fn newParser(...) !*Parser { ... }
pub fn parseArgs(...) !void { ... }

// Variables: snake_case
const my_variable = 42;
const user_input = "hello";

// Constants: SCREAMING_SNAKE_CASE
const MAX_ARGS = 100;
const DEFAULT_PORT = 8080;
```

#### Formatting

- **Indentation**: 4 spaces (no tabs)
- **Line Length**: Aim for 100 characters, hard limit at 120
- **Braces**: Opening brace on the same line
- **Spacing**: One space after keywords, around operators

```zig
// Good
pub fn example(allocator: std.mem.Allocator, value: i64) !void {
    if (value > 0) {
        const result = value * 2;
        std.debug.print("Result: {}\n", .{result});
    }
}

// Bad
pub fn example(allocator:std.mem.Allocator,value:i64)!void{
    if(value>0){
        const result=value*2;
        std.debug.print("Result: {}\n",.{result});}
}
```

#### Documentation

All public functions must have doc comments:

```zig
/// Creates a new parser with the given program name and description.
///
/// You should call this function to initialize your command-line parser.
/// The parser automatically adds a default help argument (-h, --help).
///
/// **Parameters:**
/// - `allocator`: The memory allocator to use
/// - `name`: Your program's name
/// - `description`: A brief description of your program
///
/// **Returns:** A pointer to the initialized Parser
///
/// **Example:**
/// ```zig
/// const parser = try Parser.init(allocator, "myapp", "My application");
/// defer parser.deinit();
/// ```
pub fn init(allocator: std.mem.Allocator, name: []const u8, description: []const u8) !*Parser {
    // Implementation
}
```

### Error Handling

- Use Zig's error unions (`!Type`) for functions that can fail
- Create specific error types when appropriate
- Always handle errors explicitly, avoid `try` without context in library code
- Provide meaningful error messages

```zig
// Good
pub const ParseError = error{
    UnknownArgument,
    MissingRequiredArgument,
    InvalidArgumentValue,
};

pub fn parse(self: *Parser, args: []const []const u8) ParseError!void {
    if (args.len == 0) return ParseError.MissingRequiredArgument;
    // ...
}

// Usage
try parser.parse(args) catch |err| {
    std.debug.print("Parse error: {}\n", .{err});
    return err;
};
```

### Memory Management

- Always use the provided allocator
- Document ownership and lifetime of allocated memory
- Ensure all allocations have corresponding deallocations
- Use `defer` for cleanup when possible

```zig
pub fn createList(allocator: std.mem.Allocator) !std.ArrayList([]const u8) {
    var list = std.ArrayList([]const u8).init(allocator);
    // The caller owns this list and must call deinit()
    return list;
}

// Usage
const list = try createList(allocator);
defer list.deinit();
```

### Testing

- Write tests for all new functionality
- Use descriptive test names
- Test both success and failure cases
- Include edge cases

```zig
test "Parser.flag - sets value to true when flag is present" {
    const allocator = std.testing.allocator;
    const parser = try Parser.init(allocator, "test", "Test program");
    defer parser.deinit();

    const verbose = try parser.flag("v", "verbose", null);
    
    const args = [_][]const u8{ "program", "-v" };
    try parser.parse(&args);
    
    try std.testing.expect(verbose.* == true);
}

test "Parser.flag - sets value to false when flag is absent" {
    const allocator = std.testing.allocator;
    const parser = try Parser.init(allocator, "test", "Test program");
    defer parser.deinit();

    const verbose = try parser.flag("v", "verbose", null);
    
    const args = [_][]const u8{"program"};
    try parser.parse(&args);
    
    try std.testing.expect(verbose.* == false);
}
```

## 💬 Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation changes only
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code changes that neither fix bugs nor add features
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **chore**: Changes to build process, dependencies, etc.

### Examples

```bash
# Feature
feat(parser): add support for environment variable fallback

# Bug fix
fix(validation): handle empty string in validator function

# Documentation
docs(readme): add examples for selector arguments

# Refactoring
refactor(command): simplify argument parsing logic

# Breaking change
feat(parser)!: change parse() to return error union

BREAKING CHANGE: parse() now returns ParseError instead of generic error
```

### Rules

- Use present tense ("add feature" not "added feature")
- Use imperative mood ("move cursor to..." not "moves cursor to...")
- First line should be 50 characters or less
- Reference issues and pull requests in the footer
- Mark breaking changes with `!` and `BREAKING CHANGE:` in footer

## 🔄 Pull Request Process

### Before Submitting

1. **Update from upstream**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run tests**
   ```bash
   zig build test
   ```

3. **Format code**
   ```bash
   zig fmt .
   ```

4. **Update documentation** if needed

### Submitting Your PR

1. **Push your changes**
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create Pull Request** on GitHub with:
   - Clear title following commit message guidelines
   - Description of changes
   - Reference to related issues
   - Screenshots/examples if applicable

### PR Template

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issues
Closes #123
Related to #456

## How Has This Been Tested?
Describe the tests you ran and how to reproduce them.

## Checklist
- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### Review Process

1. A maintainer will review your PR
2. Address any feedback or requested changes
3. Once approved, a maintainer will merge your PR

**Note**: Be patient! Reviews may take a few days depending on maintainer availability.

## 🧪 Testing

### Running Tests

```bash
# Run all tests
zig build test

# Run specific test file
zig test src/parser.zig

# Run with verbose output
zig build test --summary all
```

### Writing Tests

Place tests in the same file as the code they test:

```zig
// src/parser.zig

pub fn parse(...) !void {
    // Implementation
}

test "parse handles empty arguments" {
    // Test implementation
}

test "parse handles unknown arguments" {
    // Test implementation
}
```

### Test Coverage

Aim for:
- **All public functions** should have tests
- **Edge cases** should be covered
- **Error conditions** should be tested
- **Integration tests** for complex workflows

## 📚 Documentation

### What to Document

- **Public API**: All public functions, types, and constants
- **Examples**: Code examples for common use cases
- **README**: Keep README.md up to date with new features
- **Comments**: Explain complex logic or non-obvious decisions

### Documentation Style

Use clear, concise language with:
- User-focused perspective ("you", "your")
- Code examples where helpful
- Parameter and return value descriptions
- Common pitfalls or gotchas

## ❓ Questions?

If you have questions about contributing:

1. **Check existing issues** and discussions
2. **Read the documentation** in the README
3. **Open a discussion** on GitHub
4. **Ask in the issue** you're working on

We're here to help! Don't hesitate to ask questions.

---

## 🙏 Thank You!

Your contributions make argonaut better for everyone. We appreciate your time and effort in helping improve this project!

**Happy coding! 🎉**