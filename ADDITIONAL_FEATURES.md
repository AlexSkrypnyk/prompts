# Additional features in this fork

The following features have been added to this fork of Laravel Prompts:

- [Added description support](#added-description-support)
- [Added passing of the value to $validateUsing callable](#added-passing-of-the-value-to-validateusing-callable)
- [Allow static validator to run before prompt's validator](#allow-static-validator-to-run-before-prompts-validator)

## Added description support

PR: https://github.com/laravel/prompts/pull/200

This PR introduces **description** field support for all interactive Prompts
components, giving developers a way to provide helpful contextual text directly
above the main prompt content. The description is displayed within the same box,
separated by a divider line, and supports both single-line and multi-paragraph
guidance with automatic text wrapping that matches each component’s natural
width. This makes prompts more user-friendly, especially for complex forms or
guided workflows, while maintaining full backwards compatibility with existing
code.

From an implementation standpoint, the feature is built on a shared
`RendersDescription` trait to keep code consistent across all nine interactive
components (`text`, `select`, `textarea`, `password`, `multiselect`, `confirm`,
`suggest`, `search`, `multisearch`). Each component calculates widths
appropriately to ensure clean formatting, and all helper functions, builder
methods, and tests were updated accordingly. A total of 18 new tests ensure
correctness, and playground examples were enhanced to demonstrate the new
description functionality in action.


### Text Input with Description
```php
use function Laravel\Prompts\text;

$name = text(
    label: 'Project name',
    placeholder: 'my-awesome-project',
    description: 'Enter a unique name for your project. This will be used as the directory name and package identifier.',
);
```

**Visual Output:**
```
┌ Project name ─────────────────────────────────────────┐
│ Enter a unique name for your project. This will be    │
│ used as the directory name and package identifier.    │
├───────────────────────────────────────────────────────┤
│ my-awesome-project                                    │
└───────────────────────────────────────────────────────┘
```

### Select with Multi-line Description
```php
use function Laravel\Prompts\select;

$framework = select(
    label: 'Choose framework',
    options: ['Laravel', 'Symfony', 'CodeIgniter'],
    description: 'Select the PHP framework for your project. Laravel is recommended for modern web applications with extensive features.

Framework choice affects development speed and available packages.',
);
```

**Visual Output:**
```
┌ Choose framework ─────────────────────────────────────┐
│ Select the PHP framework for your project. Laravel    │
│ is recommended for modern web applications with       │
│ extensive features.                                   │
│                                                       │
│ Framework choice affects development speed and        │
│ available packages.                                   │
├───────────────────────────────────────────────────────┤
│ › Laravel                                             │
│   Symfony                                             │
│   CodeIgniter                                         │
└───────────────────────────────────────────────────────┘
```

### API Changes
```php
// Before
text(label: 'Name', placeholder: 'Enter name');

// After (backwards compatible)
text(label: 'Name', placeholder: 'Enter name', description: 'Your full name as it appears on official documents.');
```

## Added passing of the value to $validateUsing callable

PR: https://github.com/AlexSkrypnyk/prompts/pull/3

Prompt's `validate()` receives the `$value` as a first argument.

The `static::$validateUsing` override callback, however, does not receive a
`$value`.

This change aligns the behaviour by fixing the inconsistency in how callbacks
are handled.

## Allow static validator to run before prompt's validator

PR: https://github.com/laravel/prompts/pull/186

This PR changes how prompt validation is handled by giving precedence to the
static `$validateUsing` callback over the instance-level `validate()` closure.
This
allows developers, especially in testing environments, to intercept validation
results and handle failures (for example, by throwing exceptions) instead of
having tests hang waiting for interactive input.

By making the static validator run first, test frameworks like PHPUnit or Pest
can reliably assert validation outcomes without being blocked by prompt
behavior.

```php
// Fake input.
Prompt::fake(['t', 'e', 's', 't', Key::ENTER]);

// Intercept handling of the validation by throwing an exception.
Prompt::validateUsing(function (Prompt $prompt) {
    if (is_callable($prompt->validate)) {
        $error = ($prompt->validate)($prompt->value()); // <--- similar to what Prompt::validate() does.
        
        // Intercepting and throwing an exception that will later be picked up by a test and would fail an assertion.
        if ($error) {
            throw new \RuntimeException(sprintf('Validation for "%s" failed with error "%s".', $prompt->label, $error));
        }
    }

    return NULL;
  });
  
// call the prompt  
...

// Assert for the exception.
...
```

Before this PR, the static `$validateUsing` will not be assessed if the Prompt's
`validate()` closure is set, making it impossible to intercept the validation
handling.

After this PR is merged, the static `$validateUsing` will take precedence and
will allow to intercept the validation.
