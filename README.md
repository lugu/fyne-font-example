fyne-font-example
====


[Fyne](https://fyne.io) で日本語フォントを利用するサンプルアプリケーションです。

Sample application that uses different fonts in [Fyne](https://fyne.io).

> これは Fyne v2.x についての説明です. Fyne v1.x 以前のバージョンについて知りたい場合は [v1](./v1) 以下を参照してください.
>
> This is a description for Fyne v2.x. If you want to know for Fyne v1.x or earlier, please refer to the [v1](./v1) directory.

<img src="./resource/image-v2.png" width=300>

> Prefer to work with the GUI? Try the [fyne-theme-generator](https://github.com/lusingander/fyne-theme-generator)!

## Summary

#### 0. `fyne` コマンドをインストール / Install `fyne` command

```
$ go get fyne.io/fyne/v2/cmd/fyne

$ fyne
Usage: fyne [command] [parameters], where command is one of:
...
```

#### 1. フォントファイルを用意して `fyne bundle` コマンドを実行 / Prepare the font file and execute `fyne bundle` command

```
$ fyne bundle mplus-1c-regular.ttf > bundle.go

$ head -n 9 bundle.go
// auto-generated

package main

import "fyne.io/fyne/v2"

var resourceMplus1cRegularTtf = &fyne.StaticResource{
	StaticName: "mplus-1c-regular.ttf",
	StaticContent: []byte{
```

See [./v2/bundle.go](./v2/bundle.go).

> Warning: the file size is very large

#### 2. カスタムテーマを作成しフォントリソースを読み込む / Create the custom theme and load font resources

```go
type myTheme struct{}

func (*myTheme) Font(s fyne.TextStyle) fyne.Resource {
	if s.Monospace {
		return theme.DefaultTheme().Font(s)
	}
	if s.Bold {
		if s.Italic {
			return theme.DefaultTheme().Font(s)
		}
		return resourceMplus1cBoldTtf
	}
	if s.Italic {
		return theme.DefaultTheme().Font(s)
	}
	return resourceMplus1cRegularTtf
}
...
```

See [./v2/theme.go](./v2/theme.go).

#### 3. カスタムテーマを読み込む / Load the custom theme

```go
...
	a := app.New()
	a.Settings().SetTheme(&myTheme{})
...
```

See [./v2/main.go](./v2/main.go).


## もう少し詳しく

`bundle.go` は [fyne command](https://github.com/fyne-io/fyne/tree/master/cmd/fyne) を利用して生成しています.

```
$ fyne bundle mplus-1c-regular.ttf > bundle.go
$ fyne bundle -append mplus-1c-bold.ttf >> bundle.go
```

詳細については以下の記事に記載しています.

- [`fyne` コマンドで各種リソースをバンドルする方法](https://lusingander.netlify.app/posts/200613-fyne-resourece/)
- [Fyne で日本語を扱う](https://lusingander.netlify.app/posts/200614-fyne-font/)

公式のチュートリアルにもリソースのバンドルについて追記されました.

- [Bundling resources | Develop using Fyne](https://developer.fyne.io/tutorial/bundle)


## A little more details

`bundle.go` is generated using [fyne command](https://github.com/fyne-io/fyne/tree/master/cmd/fyne).

```
$ fyne bundle mplus-1c-regular.ttf > bundle.go
$ fyne bundle -append mplus-1c-bold.ttf >> bundle.go
```

See the Blog below for more information. (Japanese)

- [About the `fyne` command](https://lusingander.netlify.app/posts/200613-fyne-resourece/)
- [About fonts](https://lusingander.netlify.app/posts/200614-fyne-font/)

An official tutorial has also been added on resource bundling.

- [Bundling resources | Develop using Fyne](https://developer.fyne.io/tutorial/bundle)

----

[M+ FONTS](http://mplus-fonts.osdn.jp/) is included and used as a sample font file.

http://mplus-fonts.osdn.jp/