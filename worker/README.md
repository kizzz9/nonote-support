# nonote お問い合わせ Worker

GitHub Pages のサポートページ（kizzz9.github.io/nonote-support）のフォーム送信を受け、Resend で開発者へメール転送する Cloudflare Worker。
DebuffTimer / Tabilm / Canopy の `*-contact` と同じ構成。宛先メールはシークレット `CONTACT_TO` だけにあり、ページにもコードにも出ない。

## デプロイ（初回）

同じ Cloudflare アカウント（ID: `93a6265a7aa9a36837fa7c06a43ea51c`）に `nonote-contact` として公開する。

```bash
cd worker
export CLOUDFLARE_API_TOKEN="$(cat .cf_token)"          # 他アプリの worker/.cf_token と同じトークンでよい
export CLOUDFLARE_ACCOUNT_ID="93a6265a7aa9a36837fa7c06a43ea51c"

wrangler deploy                       # https://nonote-contact.nsmtkz9.workers.dev
wrangler secret put RESEND_API_KEY    # 他アプリと同じ Resend キー
wrangler secret put CONTACT_TO        # 届け先 Gmail（Resend 登録アドレスと同一）
wrangler secret put TURNSTILE_SECRET  # kizzz9.github.io 用ウィジェットの Secret（Site Key 0x4AAAAAADv-5iOVlinTmIvp と対）
```

`TURNSTILE_SECRET` が未設定なら Worker は Turnstile 検証を飛ばし、ハニーポット + Origin チェックだけで動く。
