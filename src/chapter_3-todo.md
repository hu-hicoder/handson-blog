# 機能追加の種類を知る

ここでは、どのように機能追加をしていけばいいかを学びます。

機能追加をするとき、大きく分けて二つの方針があります。

- コードを書く。
- 外部ライブラリを導入する。

実際は、外部ライブラリを導入したらそのライブラリを使うためにコードを書く必要がありますが、ここでは、コードを書く流れと、外部ライブラリを導入する流れを分割して紹介します。

## コードを書く

Reactの紹介で、「データを元にしてUI描画を行う」というような話をしました。コードを追加するときも、基本的には **「バックエンドサーバから得たjson形式のレスポンスを、うまく加工してUIに変換する」** という作業が多いです。

手順としては以下のようになります

- 1: バックエンドサーバとの接続がうまくいっていることを確かめる
- 2: フロントエンドでクライアントに対し何を返すべきか考える
- 3: style, UIの調整

1について、`HelloWorld.tsx` では以下の部分でバックエンドサーバとの通信を行っています

```ts
  useEffect(() => {
    const load = async () => {
      await fetch("http://localhost:3001/article") // ここでバックエンドサーバと通信
        .then((res) => res.json())
        .then((data) => setArticles(data["Articles"]))
        .catch((err) => new Error("Cannot Fetch" + err));
    }
    load()
  } , []);
```

そのため、バックエンドサーバのパス(`articles` から別のものに)を変えるだけで、最初のうちはこれをコピペしてちょっとだけ修正して使っていくのが良いと思います。

2について、 [`articles`の宣言箇所](https://github.com/hu-hicoder/blog/blob/8c46b2c1abe519114d9ade779042fc8cc0fe5a78/blog-public/frontend/src/HelloWorld.tsx#L15)から`articles`の型は `Article`の配列または`null`と分かります。そのため、以下の描画部分では、`item.ID`や`item.Author`などを使っていますが、これ以外にも `item.title` なども使うことができます。

```ts
  return (
    <>
      <h1 className="hello">Hello World!!</h1>{" "}
      {articles && articles.map(item => <div key={item.ID}>Author: {item.Author} / Content: {item.Content}</div>)}
    </>
  );
```

3について、これは[CSSを当てていく](https://ja.reactjs.org/docs/faq-styling.html)か、[ライブラリを使うことに](https://tailwindcss.com/)なります。

こうしてコードを書くとしても、どうしても個人では限界があります。そこで、フロントエンドではnpmを利用して外部ライブラリを導入することが一般的です。これを説明していきます。

## 外部ライブラリを導入する

外部ライブラリを使うときは、例えば以下のような状況が考えられます。

- markdown形式でブログを書いて、HTMLでレンダリングさせたい。 → [markdown-it](https://github.com/markdown-it/markdown-it) の利用
- useEffect周りをもっと簡潔に書きたい。特に、エラー周りをもっと楽に扱いたい。 → [swr](https://swr.vercel.app/ja) の利用
- CSSをもっと楽な方法で書きたい → [Tailwind CSS](https://tailwindcss.com/docs/guides/create-react-app)

他にも様々なライブラリがありますが、大抵は以下のコマンドで入ります。

```sh
npm install name-of-the-library # 本番環境でも使う
npm install -D name-of-the-library # 開発でしか使わない
```

ライブラリを入れたら、 `import { name-of-function } from "name-of-the-library"` をtsxファイルの中に書いて使えるようになります。

うまく行かない場合やよく分からない場合はその外部ライブラリのREADMEやdocumentを見るとよいでしょう。

## 確認テスト

- [ ] コードを書いて機能追加するときに、どこを変更していけばよいかのイメージがついた。
- [ ] 外部ライブラリが色々あることを知った。導入の仕方のイメージがついた。