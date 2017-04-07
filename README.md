# SgSDK-iOS

## SDK 支援的最低iOS版本 8.0

## SDK Sample 支援的最低iOS版本9.0

## 使用方法
  - 在Xcode內點選General標籤，找到"Embedded Binaries"
  - 點選"+"按鈕將"SgSDK.framework"加入專案
  - 加入完成後可在專案裡import SgSDK

## function
  - func SetListener(listener callBack: @escaping (SgSDKResult) -> Void)
    - 使用SgSDK第一步就是，設置listener，沒有設置的話後面的function無法使用
    - listener 參數SgSDKResult有三個屬性
      1. Code: Int 回傳Error Code
      2. Msg: String 回傳該Error Code代表的訊息
      3. Data: Any? 沒有其他資訊的話會是nil，有資訊的話可以轉換為任一結構
    ```
    SgSDK.Instance.SetListener(listener: MsgListen)
    ...
    func MsgListen(result: SgSDKResult) {
        var message = "code: \(result.Code), msg: \(result.Msg)"

        switch result.Code {
        case 1101:  // SG server validating receipt ok
            if let data = result.Data {
               tempPayresponse = data as! SgSDKPayResponse
               message.append(", receipt: \(tempPayresponse.Receipt) ")
            }
        case 1136:  // appstore transaction ok
            if let data = result.Data {
                let transaction = data as! SKPaymentTransaction
                message.append(", product ID:\(transaction.payment.productIdentifier), Date:\(String(describing: transaction.transactionDate))")
            }
        case 1141: //Restore
            if let data = result.Data {
                let productIds = data as! [String]
                for productId in productIds {
                    message.append(", restore product: \(productId) ")
                }
            }
        case 1201:  //Get Order
            if let data = result.Data {
                let payresponse = data as! SgSDKPayResponse
                message.append(", sign: \(payresponse.Sign) ")
            }
        default:
            break
        }

        self.setMessage(message)
    }
    ```
  - func Init(\_ GameKey: String, \_ AppSecret: String)
    - 使用SgSDK第二步就是Init()，沒有使用的話後面的function也無法使用
    - 初始化SgSDK, 將GameKey和AppSecret依照順序填入
    ```
    SgSDK.Instance.Init("YourGameKey", "YourAppSecret")
    ```

  - func Login()
    - 前置：Init()
    - 使用後會打開登入畫面
    - 左上角會有一個"Back"按鈕，可以回主畫面
    ```
    SgSDK.Instance.Login()
    ```
  - func Signup()
    - 前置：Init()
    - 使用後會打開註冊畫面
    ```
    SgSDK.Instance.Signup()
    ```

  - func ForgotPassword()
    - 前置：Init()
    - 使用後會打開忘記密碼的畫面
    ```
    SgSDK.Instance.ForgotPassword()
    ```

  - func ChangePassword()
    - 前置：Init()
    - 使用後會打開變更密碼的畫面
    ```
    SgSDK.Instance.ChangePassword()
    ```

  - func ParentalLock()
    - 前置：Init()
    - 使用後會打開家長操作的畫面
    - 不會有回傳
    ```
    SgSDK.Instance.ParentalLock()
    ```
  - func MyKid()
    - 前置：Init()
    - 使用後會打開新增孩童資料的畫面
    ```
    SgSDK.Instance.MyKid()
    ```
  - func MyAccount()
    - 前置：Init()
    - 使用後會打開孩童資料的畫面
    - 不會有回傳
    ```
    SgSDK.Instance.MyAccount()
    ```
  - func OpenID(listener callBack: @escaping (Any) -> Void)
    - 前置：Init()
    - 回傳：OpenID
    ```
    SgSDK.Instance.OpenID() {
            (any) -> Void in
            if let openid = any as? Int {
                print("OpenID:\(openid)")
            }
        }
    ```

  - func VerifySession(appId: String, session: String, uid: String, signature: String, listener callBack: @escaping (Any) -> Void)
    - 前置：Init()
    - Session ID登入驗證
    - 回傳：Account, OpenID, Password, EMail
    ```
    SgSDK.Instance.VerifySession(appId: "YourAppId", session: "SessionID", uid: "UID", signature: "Signature") {
            (any) -> Void in
            if let verify = any as? Verify {
                print("VerifySession, Account: \(verify.Account), OpenID: \(verify.OpenID), Password: \(verify.Password), Email: \(verify.EMail)")
            }
        }
    ```

  - public func VerifyToken(token: String, listener callBack: @escaping (Any) -> Void)
    - 前置：Init()
    - Token 登入驗證
    - 回傳：Account, OpenID, Password, EMail
    ```
    SgSDK.Instance.VerifyToken(token: "Token") {
            (any) -> Void in
            if let verify = any as? Verify {
                print("VerifyToken, Account: \(verify.Account), OpenID: \(verify.OpenID), Password: \(verify.Password), Email: \(verify.EMail)")
            }
        }
    ```

  - func Logout()
    - 前置：Init()
    - 登出
    - 不會有回傳
    ```
    SgSDK.Instance.Logout()
    ```

  - func GameStart()
    - 前置：Init()
    - 開始統計遊戲時間
    - 不會有回傳
    ```
    SgSDK.Instance.GameStart()
    ```

  - func GameStop()
    - 前置：Init()
    - 結束統計遊戲時間
    ```
    SgSDK.Instance.GameStop()
    ```

  - func GetOpenID() -> Int?
    - 前置：Login()
    - 會直接回傳結果
      - 成功的話會得到一個Int?
      - 失敗的話會得到一個nil
    ```
    if let msg = SgSDK.Instance.GetOpenID() {
            setMessage("\(msg)")
        } else {
            setMessage("Please login.")
        }
    ```

  - func GetSessionID() -> String?
    - 前置：Login()
    - 會直接回傳結果
      - 成功的話會得到一個String?
      - 失敗的話會得到一個nil
    ```
    if let msg = SgSDK.Instance.GetSessionID() {
            setMessage(msg)
        } else {
            setMessage("Please login.")
        }
    ```

  - func GetToken() -> String?
    - 前置：Login()
    - 會直接回傳結果
      - 成功的話會得到一個String?
      - 失敗的話會得到一個nil
    ```
    if let msg = SgSDK.Instance.GetToken() {
            setMessage(msg)
        } else {
            setMessage("Please login.")
        }
    ```

  - func IsLogined() -> Bool
    - 判斷是否為登入狀態
    ```
    self.setMessage("Is login? \(SgSDK.Instance.IsLogined())")
    ```

  - func GetChannelID() -> String
    - 取得平台名稱
    ```
    self.setMessage("Channel ID: \(SgSDK.Instance.GetChannelID())")
    ```

  - func ShowFloatingButton(place position: eFloatingButtonPlace = .Left)
    - 開啟懸浮按鈕
    - 前置：Init()
    - 開啟後會依照選擇的方位在視窗邊上出現
    - 按鈕可隨意拖拉，放開按鈕後會找最接近的邊吸附
    - 按鈕不動的時候10秒後會自動埋入半顆按鈕再視窗邊上
    - 按下按鈕會開啟孩童資料的畫面
    ```
    SgSDK.Instance.ShowFloatingButton(place: .Top)
    ```

  - func HideFloatingButton()
    - 關閉懸浮按鈕
    - 前置：Init()
    ```
    SgSDK.Instance.HideFloatingButton()
    ```

  - func Destroy()
    - 摧毀SgSDK.Instance
    ```
    SgSDK.Instance.Destroy()
    ```

  - func IAPInit(_ productIDs: [String])
    - 初始化In-App Purchase
    - 將商品ID輸入
    ```
    productIDs.append("ConsumbleItem")
    productIDs.append("NonConsumable")
    productIDs.append("AutoSubscription")
    productIDs.append("NonAutoSbuscriptions")
    SgSDK.Instance.IAPInit(productIDs)
    ```

  - func Pay(payRequest data: SgSDKPayRequest)
    - 購買商品
    - SgSDKPayRequest裡的ProductId必填
    - 前置：IAPInit
    ```
    SgSDK.Instance.Pay(payRequest: initPayReq(productId: "your product id", payMethod: "managed"))
    ...
    private func initPayReq(productId: String, payMethod: String) -> SgSDKPayRequest {
        let req = SgSDKPayRequest()
        req.ProductId = productId
        req.PaymentMethod = payMethod
        req.PaymentChannel = "AppStore"
        req.ServerId = "Server ID"
        req.ServerName = "Server Name"
        req.RoleId = "9487"
        req.RoleName = "Roger"
        req.RoleLevel = 99
        req.PayNotifyUrl = "PAY_NOTIFY_URL"
        return req
    }
    ```

  - func GetOrder(gameKey: String, payResponse: SgSDKPayResponse)
    - 取得訂單
    - 需要之前購買時從Sg Server拿到的SgSDKPayResponse
    ```
    SgSDK.Instance.GetOrder(gameKey: GameKey, payResponse: tempPayresponse)
    ```

  - func RestorePurchase()
    - 回復商品
    - 只有Non-Consumable, Auto-renewing subscripton這兩種商品會用到
    - 前置：IAPInit
    ```
    SgSDK.Instance.RestorePurchase()
    ```
