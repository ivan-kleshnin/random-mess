# HS / JS isomorphism

Sample backend program to calculate and output file length (in chars).<br/>
File path is passed as a first CLI argument.

---

## JS to Haskell

#### 1. Promises :suspect:
```js
main = getArgs().then(compose(readFile, head)).then(compose(print, length))
```

#### 2. Replace `then` with `chain`
```js
// Bad news: `then` conflates `map` and `flatMap` behaviors... 
main = getArgs().then(compose(readFile, head)).then(compose(print, length)) 

// it should be
main = getArgs().flatMap(compose(readFile, head)).flatMap(compose(print, length))

// or
main = getArgs().chain(compose(readFile, head)).chain(compose(print, length))
```

#### 3. Replace methods with functions
```js
main = chain(compose(print, length), chain(compose(readFile, head), getArgs()))
```

#### 4. Apply Haskell grammar
```hs
main = chain (print . length) (chain (readFile . head) getArgs)
```

#### 5. Replace `chain` with `>>=` operator
```hs
main = (>>=) (print . length) ((>>=) (readFile . head) getArgs) -- wrong argument order
```

#### 6. Fix argument order for `>>=` operator
```hs
main = (>>=) ((>>=) getArgs (readFile . head)) (print . length)
```

#### 7. Change order to normal for operators
```hs
main = getArgs >>= (readFile . head) >>= (print . length)
```

Hint: `(+) 1 2 <=> 1 + 2`

#### 8. Remove parens
```hs
main = getArgs >>= readFile . head >>= print . length
```

---

On the flip side... 

---

## Haskell to JS

#### 1. Monads :goberserk: 
```hs
main = getArgs >>= readFile . head >>= print . length
```

#### 2. Add parens
```js
main = getArgs >>= (readFile . head) >>= (print . length)
```

#### 3. Change order to normal for operators
```js
main = (>>=) ((>>=) getArgs (readFile . head)) (print . length)
```

Hint: `1 + 2 <=> (+) 1 2`

#### 4. Reverse argument order
```hs
main = (>>=) (print . length) ((>>=) (readFile . head) getArgs) -- wrong argument order
```

#### 5. Replace `>>=` operator with `chain` 
```hs
main = chain (print . length) (chain (readFile . head) getArgs) 
```

#### 6. Apply JS grammar
```js
main = chain(compose(print, length), chain(compose(readFile, head), getArgs())) 
```

#### 7. Replace functions with methods
```hs
main = getArgs().chain(compose(readFile, head)).chain(compose(print, length))
```

#### 8. Replace `chain` with `flatMap`
```js
main = getArgs().flatMap(compose(readFile, head)).flatMap(compose(print, length))
```
