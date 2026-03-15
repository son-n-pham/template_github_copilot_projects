# BDD Scenario: {ID}

| Field       | Value                  |
|-------------|------------------------|
| **ID**      | {ID}                   |
| **Status**  | New                    |
| **Created** | {ISO 8601 date}        |
| **Updated** | {ISO 8601 date}        |

## Description

{One sentence describing what behavior this scenario validates.}

## Links

| Relation | ID       | File                              |
|----------|----------|-----------------------------------|
| Parent   | {VF_ID}  | [Feature](../features/{VF_ID}.md) |
| TDD      | —        | —                                 |

## Scenario

```gherkin
Feature: {Feature name from parent}

  Scenario: {Scenario title}
    Given {initial context or state}
    When  {action or event}
    Then  {expected outcome}
    And   {optional additional assertion}
```

## Notes

{Edge cases, preconditions, or test data requirements.}
