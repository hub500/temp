## 接口文档


测试账号助记词

saddle false just round crunch soldier embrace trophy glad patrol help raise

钱包1

[86'] tb1p7ltuwf5jcsnay6f62hjdm0qnqc4mtexq4uwv4xz23y6ugd4tqafszfqe72

[84'] tb1q6x0etcn4q299az2xyaq8khn3ta4a6plu7lsjaf 有btc

钱包2 

tb1p0rps2vkq2jlzrmsv6955jvm29r9k3qj4rpl04d7cceaw3v3quxusmk92sn

钱包3

tb1pqyxzd9gc4fkv7fwaul28ya9423auj06e08pzavk0hwlu3872rgjsv82u0g


btc水龙头

https://coinfaucet.eu/en/btc-testnet/

We sent 0.01342153 bitcoins to address
tb1q6x0etcn4q299az2xyaq8khn3ta4a6plu7lsjaf


失败统一返回 code 和 msg 字段
```
{
    "code": "500",
    "msg": "receiver is exist"
}
```


1. 交易费率【GET】

http://18.143.188.255/api/order/feerate

无参数

```
type Fees struct {
	FastestFee  uint64 `json:"fastestFee"`           // 高优先级
	HalfHourFee uint64 `json:"halfHourFee"`          // 中优先级
	HourFee     uint64 `json:"hourFee"`              // 低优先级
	EconomyFee  uint64 `json:"economyFee"`           // 无优先级
	MinimumFee  uint64 `json:"minimumFee,omitempty"` // 最低优先级
}

{
   "code": "200",
   "data": {
      "fastestFee": 228,
      "halfHourFee": 224,
      "hourFee": 217,
      "economyFee": 42,
      "minimumFee": 21
   },
   "msg": "succ"
}
```


2. 钱包地址查询空投金额（订单提交前预览查看数据）【GET】

http://18.143.188.255/api/order/view?receiver=bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06

| 字段     | 类型   | 说明           |
| -------- | ------ | -------------- |
| receiver | string | 空投接受者钱包 |

```
type OrderAddResp struct {
	OrderId     string  `json:"order_id,omitempty"`  // 订单号uuid
	Collector   string  `json:"collector,omitempty"` // 收币钱包
	TransferFee float64 `json:"transfer_fee"`        // 空投手续费
	Amount      float64 `json:"amount"`              // 空投金额
	Score       float64 `json:"score,omitempty"`     // 空投积分
	FeeRate     uint64  `json:"fee_rate,omitempty"`  // 交易费率
	Claimed     uint8   `json:"claimed,omitempty"`   // 是否已领取 1=已领取
}

// 正常结果
{
   "code": "200",
   "data": {
      "transfer_fee": 0.000784,
      "amount": 8399,
      "score": 500,
      "fee_rate": 216
   },
   "msg": "succ"
}

// 没有可领取金额 【"amount": 0】
{
    "code": "200",
    "data": {
        "transfer_fee": 0,
        "amount": 0,
        "fee_rate": 249
    },
    "msg": "succ"
}

// 已领取判断 【"claimed": 1】
{
    "code": "200",
    "data": {
        "transfer_fee": 0.000207,
        "amount": 3358,
        "score": 200,
        "fee_rate": 34,
        "claimed": 1
    },
    "msg": "succ"
}
```


3. 提交订单 领取空投【POST / FormData格式提交】

http://18.143.188.255/api/order/create

| 字段     | 类型   | 说明               | 示例值                                                       |
| -------- | ------ | ------------------ | ------------------------------------------------------------ |
| receiver | string | 空投接受者钱包     | bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06 |
| feerate  | int    | 交易费率           | 120                                                          |
| inviter  | string | 邀请人钱包【可选】 | bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy88 |


```
type OrderAddResp struct {
	OrderId     string  `json:"order_id,omitempty"`  // 订单号uuid
	Collector   string  `json:"collector,omitempty"` // 收币钱包
	TransferFee float64 `json:"transfer_fee"`        // 空投手续费
	Amount      float64 `json:"amount"`              // 空投金额
	Score       float64 `json:"score,omitempty"`     // 空投积分
	FeeRate     uint64  `json:"fee_rate,omitempty"`  // 交易费率
}

{
    "code": "200",
    "data": {
        "order_id": "chbrgbldfihtqce68tog",
        "collector": "tb1pjq8k9uq7445andr6ndw6vduz6wupxa57wngxquw9pzhem6fka5nssxn442",
        "transfer_fee": 0.00048,
        "amount": 8399,
        "score": 500,
        "fee_rate": 120
    },
    "msg": "succ"
}
```

4. 查看订单空投详情信息【GET】

通过订单号来查询单个订单

http://18.143.188.255/api/order/info?receiver=bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06

http://18.143.188.255/api/order/info?order_id=chfe9uddfihn079g4740

| 字段     | 类型   | 说明           | 示例值                                                       |
| -------- | ------ | -------------- | ------------------------------------------------------------ |
| receiver | string | 空投接受者钱包 | bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06 |
| order_id | string | 订单号 | chfe9uddfihn079g4740 |


```
type OrderResp struct {
	OrderId     string    `json:"order_id"`     // 订单号uuid
	Collector   string    `json:"collector"`    // 收币钱包
	TransferFee float64   `json:"transfer_fee"` // 空投手续费
	Receiver    string    `json:"receiver"`     // 空投者钱包
	Amount      float64   `json:"amount"`       // 空投金额
	FeeRate     uint64    `json:"fee_rate"`     // 交易费率
	Result      string    `json:"result"`       // create,paying,pending,confirm,succ,fail 结果
	TxnHash     string    `json:"txn_hash"`     // 交易hash
	CreateTime  time.Time `json:"create_time"`  // 创建时间
}

// create=订单创建成功 未支付
// paying=已支付 待链上6个块确认
// pending=支付成功 待转账
// confirm=已转账 待链上6个块确认
// succ=转账成功
// fail=转账失败

{
    "code": "200",
    "data": {
        "order_id": "chbrgbldfihtqce68tog",
        "collector": "tb1pjq8k9uq7445andr6ndw6vduz6wupxa57wngxquw9pzhem6fka5nssxn442",
        "transfer_fee": 0.00048,
        "receiver": "bc1pxaneaf3w4d27hl2y93fuft2xk6m4u3wc4rafevc6slgd7f5tq2dqyfgy06",
        "amount": 8399,
        "fee_rate": 120,
        "result": "create",
        "txn_hash": "",
        "create_time": "2023-05-07T22:39:43+08:00"
    },
    "msg": "succ"
}
```

每次查看 result 和 txn_hash 字段可能会变

前端拼接后 浏览器新标签打开

BTC主网

https://blockstream.info/tx/{txn_hash}

BTC测试链

https://blockstream.info/testnet/tx/{txn_hash}


5. 空投进度条

http://18.143.188.255/api/order/progress

```
{
    "code": "200",
    "data": {
        "claim": 0, // claim + pool 之和
        "pool": 0, // 交易池中金额
        "succ": 0, // 已成功空投金额
        "total": 16800000 // 总空投金额
    },
    "msg": "succ"
}
```
