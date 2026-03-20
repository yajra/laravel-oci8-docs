# Contributing

We welcome contributions from the community! This guide explains how to get involved with Laravel-OCI8 development.

<a name="getting-started"></a>
## Getting Started

1. **Fork the repository** on [GitHub](https://github.com/yajra/laravel-oci8)
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/your-username/laravel-oci8.git
   ```
3. **Install dependencies**:
   ```bash
   composer install
   ```
4. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```


<a name="pull-request-guidelines"></a>
## Pull Request Guidelines

<a name="code-standards"></a>
### Code Standards

All contributions must follow the [PSR-12 Coding Standard](https://www.php-fig.org/psr/psr-12/). The easiest way to ensure compliance is to use PHP Code Sniffer:

```bash
# Install PHP Code Sniffer
composer require squizlabs/php_codesniffer --dev

# Check code style
./vendor/bin/phpcs --standard=PSR12 src/

# Auto-fix issues where possible
./vendor/bin/phpcbf src/
```


<a name="documentation"></a>
### Documentation

- Update the `README.md` and any relevant documentation files when changing behavior
- Add docblock comments for new public methods and classes
- Include code examples for new features


<a name="versioning"></a>
### Versioning

We follow [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** version for incompatible API changes
- **MINOR** version for new functionality in a backwards compatible manner
- **PATCH** version for backwards compatible bug fixes

Do not introduce breaking changes without a major version bump.


<a name="commit-history"></a>
### Commit History

- Keep commits focused and atomic (one logical change per commit)
- Write clear, descriptive commit messages
- Squash intermediate commits before submitting your PR


<a name="testing"></a>
## Testing

Before submitting a PR, ensure all tests pass:

```bash
composer test
```


<a name="submitting-your-pr"></a>
## Submitting Your PR

1. Push your changes to your fork
2. Open a Pull Request against the `master` branch of the main repository
3. Fill out the PR template with:
   - A clear description of the change
   - Reference any related issues
   - Steps to test the change


<a name="reporting-issues"></a>
## Reporting Issues

- Use the [GitHub Issues](https://github.com/yajra/laravel-oci8/issues) page
- Search existing issues before creating a new one
- Include your environment details (PHP version, Laravel version, Oracle version)
- Provide a minimal reproduction case for bugs

## Questions?

Feel free to open an issue for questions about the codebase or development process.

**Happy coding!**
