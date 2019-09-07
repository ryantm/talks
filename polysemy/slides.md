---
title: Polysemy
author: Ryan Mulligan
patat:
  wrap: true
...

# Polysemy

polysemy is a library for writing high-power, low-boilerplate, zero-cost, domain specific languages. It allows you to separate your business logic from your implementation details. And in doing so, polysemy lets you turn your implementation code into reusable library code.

---

# Why?

- Easy to test
- Isolates effects from core logic
- Easier to use than transformers
- Allows multiple instances of the same effect monads without wrappers

---

# Why not?

- Slightly harder to set up than transformers
- Compiler error messages are more cryptic

---

# Parts

- GADT for tracking effects
- `makeSem` Template Haskell
- effect interepters

---

# Example

```haskell
oneHourAgo :: IO UTCTime
oneHourAgo = getCurrentTime <&> addUTCTime (fromInteger $ -60 * 60)
```

How do we test this easily? Use polysemy


---

# GADT

```haskell
data Time m a where
  Now :: Time m UTCTime
```

---

# makeSem

```
makeSem ''Time
```

---

# Interpreters: IO

```Haskell
runIO :: Member (Lift IO) r => Sem (Time ': r) a -> Sem r a
runIO =
  interpret $ \case
    Now -> sendM getCurrentTime
```

---

# Interpreters: Pure

```Haskell
runPure :: UTCTime -> Sem (Time ': r) a -> Sem r a
runPure t =
  interpret $ \case
    Now -> pure t
```

---

# Using it

```Haskell
oneHourAgo :: Member Time r => Sem r UTCTime
oneHourAgo = now <&> addUTCTime (fromInteger $ -60 * 60)
```

---

# Now we can test it

```Haskell
$setup
>>> let exampleCurrentTime = parseTimeOrError False defaultTimeLocale "%Y-%-m-%-d" "2019-06-06" :: UTCTime

Examples:

>>> run $ runPure exampleCurrentTime oneHourAgo
2019-06-05 23:00:00 UTC
```

---

# Running it in IO

```Haskell
runM $ runIO oneHourAgo
```

---

# Find out more

https://github.com/isovector/polysemy
