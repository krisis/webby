# Webby

An easy to use Haskell web-server inspired by Scotty.

# Build

Clone the repo and run `stack build`

# Example

``` haskell
module Main where

import qualified Data.Text                as T
import qualified Network.Wai.Handler.Warp as W
import           UnliftIO                 (liftIO)
import qualified UnliftIO.Exception       as E

import           Webby

main :: IO ()
main = do
    let routes = [ get "/api/a" (text "a")
                 , get "/api/b" (text "b")
                 , post "/api/capture/:id" (do idVal :: Int <- getCapture "id"
                                               text $ T.pack $ show idVal
                                           )
                 , get "/api/showEnv" (do env <- getAppEnv
                                          json env
                                      )
                 , get "/aaah" (liftIO $ E.throwString "oops!")
                 ]

    webbyApp <- mkWebbyApp (3::Int) routes
    putStrLn "Starting webserver..."
    W.runEnv 7000 webbyApp
```

You can try the example above, by cloning the repo and running the
example:

``` shell
examples/Basic.hs
```
