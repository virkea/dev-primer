# Backend Platform Guide

## 3rd Party Libraries

> When to use 3rd party libraries, when to build it by ourselves?

If we use open source 3rd party library, we have to be ready to maintain it and not relying to its original developer. The rule of thumb is: if our requirements are mission critical or many customizations are needed, then it is better to build the functionality in-house.

## ActiveRecord

- There are two validations that must be created (duplicated) as both `ActiveRecord::Validation` and database constraints: **presence** and **uniqueness**
- Validations should be written on per attribute basis, avoid the usage of `validate_presence_of`, `validate_uniqueness_of`, etc
- When creating, updating or deleting existing seeds, we should also create seed migrations. After the seed migrations are executed on production, we can then move it into the `applied` folder.
- Index should be named properly, e.g:
```ruby
  ix_table_names_on_column1_and_column2
  ix_table_names_on_columnable # for polymorphic
```
- Add `ctz_` prefix for non-standard tables & models (e.g. models that only utilized by a specific client)
- Difference between `seq` and `order_seq` when representing sequence on a model
  - `seq` means the sequence must be present (null: false) and unique
  - `order_seq` meants the sequence can be null and doesn't have to be unique
- When writing validations, relations in models and its specs, make sure that their sequence is the same. It is very helpful when reviewing the codes.
- Put `belongs_to` on top of `has_many`

### GridProcessor

- If a controller has the same name with model, aforementioned model attributes on `column_defs` and `field_lookup` doesn't need model name prefix

## ActionController

- If a controller has the same name with model, its behaviour must be predictable (CRUD)

## VERP

- Don't put classes in client repository, which:
  1. Already exist in core, and 
  2. Doesn't need customization
- Make sure which codes should be put on core and which code should be put on client repository (before merging or accepting pull request, we have to ensure that)

## Git

- Learn `git cherry-pick`
- Version numbering
- Naming of commit, use this format: `EPROC-XYZ #comment <yourcomment>`
- When to create branch from `staging`, when from `master`, when from any other branch

## Code Reviewing

- Reviewing code should start from the lowest level, and that means database, move up to API, then views / serializers.

## Maintainance

- Identify maintenance overhead, which area that:
  - Client can do by themselves
  - Analyst or support team can do it
  - Developers have to do it

  It is better if developers can move the overhead up.
- Classify which features can be done by support developers and which features have to be done by feature developers
