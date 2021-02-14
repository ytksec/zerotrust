# ゼロトラストアーキテクチャの概要、移行方法等

2020年8月、米国国立標準技術研究所（NIST）が「SP800-207 ゼロトラスト・アーキテクチャ」を正式公開しました。  
以下に７つの基本原則を記載します。

## NISTによるゼロトラストの基本７原則

+ データソースとコンピュータサービスは、全てリソースと見なす
+ 「ネットワークの場所」に関係なく、通信は全て保護される
+ 組織のリソースへのアクセスは、全て個別のセッションごとに許可される
+ リソースへのアクセスは動的なポリシーによって決定される
+ 組織が保有するデバイスは、全て正しくセキュリティが保たれているように継続的に監視する
+ リソースの認証と認可は、全てアクセスが許可される前に動的かつ厳密に実施される
+ 資産・ネットワーク・通信の状態について可能な限り多くの情報を収集し、セキュリティを高めるために利用する

## 主要コンポーネント

+ IDaaS(Identity as a Service)
  + ID管理、認証機能をクラウドで提供している製品。
  + Okta、OneLogin、AzureADあたりが有名

+ EDR(Endpoint Detection and Response)
  + PC、サーバー、スマートフォン、タブレットなどのネットワークに接続されているエンドポイントの操作や動作の監視を行い、サイバー攻撃を受けたことを発見し次第対処するソフトウェアの総称。  
  + Crowdstrike、Carbon Black、Cybereason、Sophosあたりが有名

+ SWG(Secure Web Gateway)
  + 外部へのアクセスを安全に快適な状態で行うためのクラウド型プロキシ。  
  + Zscaler、Netskope、iboss、Cisco Umbrellaあたりが有名

+ CASB(Cloud Access Security Broker)
  + ユーザー（企業）と複数のクラウドプロバイダーの間に単一のコントロールポイントを設けて、クラウドサービスの利用状況を可視化/制御することで、一貫性のあるセキュリティポリシーを適用するクラウドゲートウェイ。  
  + SWGとセットで提供されている場合が多い。  
  + Netskope、McAfee MVISION CLOUDあたりが有名

+ IAP(Identity Aware Proxy)
  + リモート環境からオンプレミスやプライベートクラウドへのアクセスをする際にユーザーとアプリケーションの間に入って通信を仲介するプロキシ。

## 一般的ゼロトラスト ネットワークへの移行手順

1. ID整備（IDaaS導入）

ゼロトラスト のポイントは、適切なエンティティ（人、場所、デバイス）が適切なリソース（企業アプリケーション、認可SaaS等）にアクセスするという点になりますが、その際にID認証を行うという点が非常に重要になります。  
Okta,Onelogin,AzureADなどが有名ですが、まずはこのID整備を行うのをオススメいたします。

2. エンドポイント保護（EDR導入）

次にエンドポイント保護になります。昨今リモートワークが急激に増え、会社のPCやスマートフォンを自宅やカフェなどのWifi等のネットワークに接続する機会が増えたのではないかと思います。  
それらのネットワークは会社からすると管轄外のネットワークになり、ウィルス感染する可能性が増えてくるのではないかと思います。  
そういった場合に有効なのがエンドポイント保護。  
EDR(Endpoint Dection and Response)製品を導入し、未知のウィルス感染を防ぐことを推奨します。  
これから働く場所が問われない働き方が当たり前になるため、エンドポイント保護は欠かせません。  

3. 出口対策（SWG、CASB導入）

現在ほとんどの企業ネットワークが境界型ネットワークと言われており、本社、データセンターやキャリア契約のFirewallなど一箇所からインターネットへ出ている形だと思います。  
上記のように働く場所が事務所以外が増えてきたのにも関わらず、必ず企業ネットワークにVPNなどで入り、そこからインターネットへ接続するのは通信的に非効率であり、企業ネットワークの回線も混み合うという問い合わせが非常に増えているのが現状です。  
そこでクラウド型のゲートウェイであるSWG(Secure Web Gateway)やCASB(Cloud Access Decurity Broker)を導入することを推奨いたします。  
クラウド型であるため働く場所がどこであれ効率的な通信をすることが可能で、通信が混み合うこともまず無いです。  
導入には、企業ネットワークのインターネット向け通信をローカルブレイクアウトさせる必要があります。  
また、テレワークで従業員が業務に関係ないサイトを閲覧していないかといった不安も、SWGやCASBのポリシー設定をすることで防ぐことができたり、アラートやログ管理にも利用することができます。  
もし業務端末がサイバー攻撃者のC&Cサーバにアクセスしようとする際に防ぐことも可能であるため、出口対策と言える製品になります。  
主要な製品はZscaler、Netskopeになります。

4. 脱VPN、脱閉域網（IAP、SDP導入）

このフェーズまでくると、本格的にゼロトラスト ネットワークとなって参ります。従業員のインターネット通信が企業ネットワークからローカルブレイクアウトする環境になると、いよいよ契約している閉域網の帯域が余ってきたり、VPN自体を無くしていきたいという方向になってくるのではないかと思います。  
その際に導入をオススメするのがIAP(Identity Aware Proxy)、SDP(Software Defined Perimeter)製品となります。  
これらは企業ネットワーク（オンプレミスやパブリッククラウドなど）で利用しているアプリケーション宛ての通信をクラウド経由でプロキシしてくれる製品であり、通信効率も良くなると言われています。  
これらの製品を活用することで、VPNや閉域網をなくすことができ、企業ネットワークへの投資額を大幅に削減することが可能となります。  
*一つ注意点が、サーバのパッチ配信など、サーバ発信となる通信はこれらの製品ではクライアントへ通信が出来ないため、安価なインターネットVPNで企業ネットワークを構築しておく必要があります。*  
もしくは、次のステップのMDM、UEM製品を導入したり、アプリケーション側の設定で、必ずクライアント発信とすることで上記製品がフル活用出来てくると思います。  
主要な製品は、Zscaler、Netskope、AppgateSDPあたりとなります。  

※エンドポイント管理（MDM,UEM導入）

どこでも働ける環境となり、端末を持ち歩くことが当たり前となる中でエンドポイント管理は必要です。  
エンドポイント管理については特に導入の順番は問わず、導入できるタイミングで実施することをオススメしますが、ID環境が整備されてからの方が手戻りはないのではないかと思います。
導入後、リモートワイプ出来る機能はもちろんのこと、端末へのパッチ配信や制御などが可能となります。  
端末管理にも優れており、WindowsUpdateやブラウザのバージョン、インストール済みのウィルス対策製品のバージョン管理も行う製品もあるため、セキュリティポリシーを統一していくことが出来るのではないかと思います。  
主要な製品は、MobileIron、Microsoft Intuneあたりとなります。  
