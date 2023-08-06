# Air-Quality-Analysis(空氣品質分析)

本專案是一個關於空氣品質偵測的應用，並提供相關資訊在網頁上展示。該專案使用各種感測器和資料來源，並利用機器學習模型來預測空氣品質指標。

## 功能特點

- 監測空氣品質指標：該專案使用感測器來測量空氣品質指標，例如PM2.5、PM10、二氧化氮（NO2）、一氧化碳（CO）等。這些感測器可以連接到嵌入式系統或單獨的硬體裝置。
- 數據收集和儲存：專案會定期收集感測器數據，並將其儲存在資料庫中。這樣可以建立歷史數據庫，以便後續分析和可視化。
- 機器學習模型：專案使用機器學習模型來預測空氣品質指標的變化趨勢。模型可以根據過去的數據和其他相關特徵進行訓練，並用於未來的預測。
- 網頁展示：專案提供一個網頁界面，用於顯示空氣品質指標的數據和預測結果。該界面可以呈現圖表、圖形和其他可視化方式，以方便用戶理解和分析數據。

## 技術組件

這個專案包含以下技術組件：

- **感測器和數據收集**：使用各種感測器來監測空氣品質指標，例如空氣品質監測器、溫度和濕度感測器等。感測器數據通常通過嵌入式系統或單獨的硬體裝置進行收集。
- **資料庫**：使用一個資料庫系統（例如MySQL、PostgreSQL等）來儲存感測器數據和其他相關資訊。這樣可以方便地進行後續的數據管理和分析。
- **機器學習模型**：使用機器學習算法，如回歸模型、時間序列模型等，來預測空氣品質指標的變化趨勢。模型可以使用Python的機器學習庫（例如scikit-learn、TensorFlow等）來實現。
- **網頁展示**：使用網頁開發技術（如HTML、CSS、JavaScript等）來構建一個用於顯示空氣品質數據和預測結果的網頁界面。可以使用一些JavaScript圖表庫（如Chart.js、D3.js等）來實現數據的可視化。

## 材料(一台機器所需的設備)
- 開發版：ESP8266(Nodemcu V2)
- 臭氧感測器：MQ-131
- PM2.5感測器：PMS5003
- 溫溼度感測器：DHT11(也可使用DHT22，但程式碼上要改參數)
- 太陽能板(含12V輸出)
- 電量顯示器(可有可無)
- OLED顯示器:SSD1306
- 穩壓IC X2(盡量選功耗低的)

## 電路
<img src="./circuit.svg">

## 安裝和使用

以下是在本地環境中運行該專案的基本步驟：

1. 克隆該專案的GitHub存儲庫到本地機器。
2. 配置和設置感測器和相關硬體（如果適用）。
3. 創建一個虛擬環境，並安裝所需的依賴庫。建議使用`pip`來管理Python依賴項。
4. 配置資料庫，並確保能夠正確連接到資料庫系統。
5. 訓練機器學習模型，並保存模型以供後續使用。
6. 運行專案主程序，開始收集感測器數據，進行預測和數據展示。

請參考專案中的更詳細的文件和說明，以獲得有關安裝和使用的進一步指引。
