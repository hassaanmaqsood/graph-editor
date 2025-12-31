# Contributing to Graph Editor

First off, thank you for considering contributing to Graph Editor! 🎉

## Code of Conduct

This project and everyone participating in it is governed by respect, kindness, and professionalism. By participating, you are expected to uphold this standard.

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check the existing issues to avoid duplicates. When you create a bug report, include as many details as possible:

- **Use a clear and descriptive title**
- **Describe the exact steps to reproduce the problem**
- **Provide specific examples** (code snippets, screenshots)
- **Describe the behavior you observed and what you expected**
- **Include browser/OS information**

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion:

- **Use a clear and descriptive title**
- **Provide a detailed description** of the suggested enhancement
- **Explain why this enhancement would be useful**
- **Provide examples** of how it would work

### Pull Requests

1. Fork the repo and create your branch from `main`
2. If you've added code, add tests if applicable
3. Ensure your code follows the existing style
4. Update documentation as needed
5. Write clear commit messages
6. Open a Pull Request with a clear description

## Development Setup

```bash
# Clone your fork
git clone https://github.com/yourusername/graph-editor.git
cd graph-editor

# Create a branch
git checkout -b feature/my-feature

# Make your changes...

# Test your changes by opening the demo files in a browser
# Open demo-unified.html, keyboard-zoom-demo.html, etc.

# Commit and push
git add .
git commit -m "Add feature: description"
git push origin feature/my-feature
```

## Coding Guidelines

### JavaScript Style

- Use ES6+ features
- Use descriptive variable names
- Add JSDoc comments for public methods
- Keep functions focused and small
- Avoid global variables

### Example:

```javascript
/**
 * Add a new node to the graph
 * @param {Object} nodeData - Node configuration
 * @returns {HTMLElement} The created node element
 */
addNode(nodeData) {
  // Implementation
}
```

### CSS Style

- Use CSS custom properties for theming
- Follow BEM-like naming for clarity
- Keep specificity low
- Group related properties

### Commit Messages

- Use present tense ("Add feature" not "Added feature")
- Use imperative mood ("Move cursor to..." not "Moves cursor to...")
- Limit first line to 72 characters
- Reference issues and PRs liberally

### Examples:

```
Add keyboard navigation for node selection
Fix edge rendering when zoomed out
Update documentation for styling API
```

## Testing

While we don't have automated tests yet (PRs welcome!), please test your changes:

1. Open all demo files in different browsers
2. Test keyboard shortcuts
3. Test touch interactions on mobile
4. Test with different themes
5. Check console for errors

## Documentation

- Update README.md if adding features
- Add JSDoc comments to new methods
- Update STYLING.md for new CSS variables
- Add examples to EXAMPLES.md

## Release Process

Maintainers handle releases:

1. Update version in package.json
2. Update CHANGELOG.md
3. Create GitHub release
4. Tag the release

## Questions?

Feel free to:
- Open an issue for discussion
- Start a GitHub Discussion
- Reach out to maintainers

## Recognition

Contributors will be added to the README.md contributors section.

Thank you for making Graph Editor better! 🚀
