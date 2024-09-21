---
title: Solidity - 101  1 ~ 4 單元

---

---
lang: zh-tw
---


Solidity - 101  1 ~ 4 單元
===
:::info
:date: 2024/09/23
:::

[TOC]

## 型別

#### 1. 整數
* int 整數包括負數
* uint 無符號 只有正整數
* uint256 無符號且有256位

#### 2. 地址類型
* address: 普通地址，可儲存20字節的值(以太坊地址的大小)
* payable address: 多加上了payable的語法，比普通地址多了 **transfer** 和 **send**，用於接收或轉帳

#### 3. ENUM
* 不只Solidity，在Angular中也有存在enum，用於提高可讀型與維護，分配名稱之用途。
    ```xml
    // 用enum将uint 0， 1， 2表示为Buy, Hold, Sell
    enum ActionSet { Buy, Hold, Sell }
    // 创建enum变量 action
    ActionSet action = ActionSet.Buy;
    ```

## Remix
####    介紹
* 是一個相對其他IDE來說，較為簡單的開發工具，適合Solidity使用，能夠更值觀的部屬與查看compiler後的情況。

![image](https://hackmd.io/_uploads/HygnZ426C.png)


## 函數
這應該算是 `重中之重` 任何的邏輯都離不開function，每個語言的funciton特性都不一樣，先上範例程式碼(不是正規，重點在先瞭解)
```xml
function <function name>(<types parameter>) {internal|external|public|private} [pure|view|payable] [returns (<return types>)]
```
1. `function` : 宣告為函數
2. `function name` : 函數名稱
3. `<types parameter>` : 函數帶入的參數為先行別後名稱 EX:`uint8 test`
4. `{internal|external|public|private}`
    * `internal` : 只能從合約內部使用，意思是如果在A合約內宣告一個B fun，那我只能在A合約內使用，只要與A合約有繼承關係也皆可使用。
    * `external` : 只能從合約外部使用，與 `internal` 相反。
    * `public` : 共有，大家都可以使用。
    * `private` : 只能合約內部使用，與 `internal` 類似，但差別是繼承關係的無法使用。
    :::info
    :bulb: **Tips** : public | private | internal 也可用于修饰状态变量。public变量会自动生成同名的getter函数，用于查询数值。未标明可见性类型的状态变量，默认为internal
    :::
5. `[pure|view|payable]`
    * `pure` : 不能讀取鏈上也不能寫入。
    * `view` : 只能讀取鏈上，不會對鏈上有人和寫入的動作。
    * `payable` : 意思是此function是一個可以轉入ETH的，能進行接收與轉帳。
    :::info
    :bulb: **Tips** : pure|view pure與view的差別在於能不能讀取鏈上資訊，為何需要這兩個?
    因為在鏈上任何操作都是需要gas fee的，下方是大致需要費用的使用。
    :::
    ---
        5-1. 寫入狀態到鏈上。
        5-2. 釋放事件。
        5-3. 創建合约。
        5-4. 使用 selfdestruct.
        5-5. 通过调用发送以太币。
        5-6. 调用任何未标记 view 或 pure 的函数。
        5-7. 使用低级调用（low-level calls）。
        5-8.  使用包含某些操作码的内联汇编。
    ---

6. `[returns ()]` : 規定函數返回的型別，下方例子代表只能返回為uint8的參數。
    ```xml
    returns (uint8)
    ```
    * 命名式返回
        ```xml
        function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
            _number = 2;
            _bool = false;
            _array = [uint256(3),2,1];
        }
        ```
        這樣就不需要在內部邏輯加上return也可以返回值。

    * 結構式賦值
        * 使用`,`隔開，會自動辨別傳入得值
        ```xml
        uint256 _number;
        bool _bool;
        uint256[3] memory _array;
        (_number, _bool, _array) = returnNamed();
        ```
        * 若為空白，就不讀取空白處返回的值
        ```xml
        (, _bool2, ) = returnNamed();
        ```
        下方是執行結果，那`(, _bool2, )`的情況，他只會把false帶入給_bool2。
        ![image](https://hackmd.io/_uploads/HkSkZH260.png)

        