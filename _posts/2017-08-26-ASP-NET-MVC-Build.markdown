---
layout: single
title: ASP.NET MVC 架設
date: 2017-08-25 17:52:37
excerpt: 了解MVC架構，並運用.Net進行簡單實作。
categories:
- study
tags: 
- ASP.net 
- MVC
---

## 前置作業
1. SQL Server
    * 連結伺服器
2. Microsoft Visual Studio
    * 建立專案，搜尋MVC，C#-MVC

前置結束可以準備開發Web囉~~

## 基本了解
### 關於MVC架構
MVC架構模式，把系統分成三塊，分別為:Model、View、Controller。
1. 控制器(Controller)：負責處理及轉發要求(Request)，可以視情況呼叫Model拿資料，也視情況呼叫View來回應，而控制器中會包含多個動作(Action)。
2. 模型(Model)：包含所有的邏輯、物件，內容豐富。
3. 檢視(View) ：專門展示處理結果給使用者，提供UI。

傳遞資料如下圖所示：
![](https://2.bp.blogspot.com/-YUWtsOlOtQY/Vz58E8CMBOI/AAAAAAAAbiQ/eXGYjaWnA9kDZZ0ESTeMiuJy2a__ZVdwQCLcB/s640/001.png)

### ASP.NET MVC 的方案目錄結構如下圖
* App_Data - 預設ASP.NET User的本機資料庫檔案
* AppStart - 網站起始設定
* Content - 存放css檔案 (*.css)
* Controller - 所有控制器的原始碼(*.cs)
* fonts- 字型
* Models - 與模型相關的原始碼
* Scripts - 存放JavaScript檔案(*.js)
* View - 所有檢視的原始碼，依據不同的Controller名稱會有對應名稱的目錄

## 關於修改與新增MVC架構
### 修改View
1. Views->Shared->_Layout.cshtml
    * 原本
	
		```
		@Html.ActionLink("應用程式名稱", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
		```
	
	
    * 更改為
	
		```
		@Html.ActionLink("Janis隨興練習網站", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
		```
	
    * 顯示結果
    ![](https://i.imgur.com/TJngfLT.png)
 
### 新增View
1. 將Janis_Route中的Index，新增檢視
    * ![](https://i.imgur.com/nLps363.png)
2. 使用版面配置頁
    * ![](https://i.imgur.com/zMCpTDz.png)
3. 新增完會在Views->Janis_Route新增一個Index.cshtml檔案
4. 範例練習
    * Views->Janis_Route->Index.cshtml
	
    ```
    @{
        ViewBag.Title = "Index";
        Layout = "~/Views/Shared/_Layout.cshtml";

        //宣告通行證列表
        IList<Pass.Models.Janis_Route.TempPass> passList =
            new List<Pass.Models.Janis_Route.TempPass>();

        passList.Add(new Pass.Models.Janis_Route.TempPass
        {
            Pass_no = 1,
            habor = "高雄"
        });

        passList.Add(new Pass.Models.Janis_Route.TempPass
        {
            Pass_no = 2,
            habor = "台中"
        });

        passList.Add(new Pass.Models.Janis_Route.TempPass
        {
            Pass_no = 3,
            habor = "台北"
        });
    }

    <h2>通行證列表</h2>
        <table class="table">
        <thead><tr><td>通行證號</td><td>港區</td></tr></thead>
        <tbody>
        @foreach(var ps in passList)
        {
            <tr>
                <td>@ps.Pass_no</td>
                <td>@ps.habor</td>
            </tr>
        }
        </tbody>
        </table>
    ```
	
    * 新增Models->Janis_Route->TempPass.cs
	
    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;

    namespace Pass.Models.Janis_Route
    {
        public class TempPass
        {
            public int Pass_no { get; set; }
            public string habor { get; set; }
        }
    }
    ```
	
    * 圖示
    ![](https://i.imgur.com/JINYl3l.png)

### 修改路由
1. Controllers->HomeContriller.cs
    * 新增Janis_index的Action
	
        ```
        public ActionResult Janis_index()
        {
            return Content(
                "<html><p>安安安安安安安~~~大家好阿阿~~</p></html>"
                );
        }
        ```
		
        ![](https://i.imgur.com/WaFFpVu.png)
		
### 新增路由
1. **新增一個Controller叫做Janis_Route**，在Controller資料夾右鍵->加入->控制器->選擇MVC5控制器-空白
    * 在不接受任何參數的狀況，url為 http://localhost:59612/Janis_Route/Index
        
		```
         public ActionResult Index()
        {
            return Content("在不接受任何參數的狀況");
        }
        ```
		
    * 在單變數的狀況，url為 http://localhost:59612/Janis_Route/Index2/11
        
		```
		public ActionResult Index2(string id)
		{
			//在單變數的狀況
			return Content(
				String.Format("單變數的狀況下，id的值為:{0}",id)
				);
		}
		```
		
	* 在多變數的狀況，url為 http://localhost:59612/Janis_Route/Index3/?id=123&id2=456
        
		```
		 public ActionResult Index3(string id, string id2)
		{
			//在多變數的狀況
			return Content(
				String.Format("多變數的狀況下，id的值為:{0} id2的值為:{1}", id, id2)
				);
		}
		```

參考資料：http://ithelp.ithome.com.tw/articles/10158283
