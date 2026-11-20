# The MaybeT Monad Transformer
The Monad Transformer allows us to **use multiple Monads together**.

## Key ideas
- In our example, we need two monads: Maybe (to handle failure), and IO (to allow us to find files and print to the console.
```
type MaybeIO a = MaybeT IO a
```
- Every monad transformer provides a lift function to ’promote’ an action in the inner monad to the combined monad.
```
class MonadTrans t where
lift :: (Monad m) => m a -> t m a
```

## Example: File Summariser
```
type Summariser a = WriterT [Summary]
					(ExceptT SummariserError IO)
					a
```
- Requires ExceptT, WriterT, and IO

## Accessing the result:
```
runWriter :: Writer w a -> (a, w)
==
runWriterT :: WriterT w m a -> m (a, w)
```
- the run functions (runwriterT, runExceptT, runReader, etc.) Essentially peel off outer layers until we reach IO

## What if we swapped layers?
The main difference in swapping layers is a different order of evaluation

# Pure Monad Transformers
Monad Transformers do not have to perform IO operations.
- For example, we can make a monad transformer that allows us to use the Writer monad for logging, but also supports potentially-failing computations
```
type WriterMaybe a =
WriterT [Summary] (MaybeT Identity) a
```

- Some of our favourite monads are actually just abbreviations of transformers
```
Maybe a = MaybeT Identity a
Except e a = ExceptT e Identity a
State s a = StateT s Identity a
```

Monad transformers are widely used in the wild:
```
Pandocs md writer:
type MD m = ReaderT WriterEnv (StateT WriterState m)
```
https://github.com/jgm/pandoc/blob/main/src/Text/Pandoc/Writers/Markdown/Types.hs

```
XMonad Window Manager:
newtype X a = X (ReaderT XConf (StateT XState IO) a)
```
https://github.com/xmonad/xmonad/blob/master/src/XMonad/Core.hs