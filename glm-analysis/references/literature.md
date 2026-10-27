# 方法對應的參考文獻

本檔列出本 skill 各階段用到的統計方法與其文獻依據；每筆後面的 `[@key]` 是 `assets/references.bib` 中的引用鍵，Quarto 報告內文直接寫 `[@key]` 即可引用。每一筆都已對照出版社、期刊、套件官方引用檔或圖書館目錄確認作者、年份、題名與卷期頁碼；DOI 只在查證時實際看到才列出，沒有看到的就不寫。

## 使用規則

1. 報告的「方法」或「參考文獻」章節，**只引用這份清單中的文獻，且只引用該次分析實際用到的方法**（例如沒有做交叉驗證，就不列交叉驗證的文獻）。
2. **不得引用本清單以外的文獻，也不得自行補寫或修改作者、年份、卷期頁碼、DOI**。需要清單沒有的文獻時，向使用者說明「清單中沒有對應文獻」，不要憑記憶產生。
3. R 套件的引用以使用者環境實際安裝的版本為準：在 R 執行 `citation("套件名")` 取得正確的年份與版本，不要沿用本檔的版本號。

## 1. GLM 基本架構（family、link、偏差）

- Nelder, J. A., & Wedderburn, R. W. M. (1972). Generalized linear models. *Journal of the Royal Statistical Society, Series A*, 135(3), 370–384. https://doi.org/10.2307/2344614 `[@nelder1972]`
- McCullagh, P., & Nelder, J. A. (1989). *Generalized Linear Models* (2nd ed.). Chapman & Hall. ISBN 9780412317606 `[@mccullagh1989]`
- Agresti, A. (2013). *Categorical Data Analysis* (3rd ed.). Wiley. ISBN 9780470463635 `[@agresti2013]`

  用於：Stage 2 依反應變數選 family／link、Gamma 的變異數與平均數平方成比例、offset、Stage 3 係數在 link scale 上的解釋。（Agresti 3rd ed. 版權頁為 2013，書商上架日為 2012 年 12 月。）

## 2. 二元資料：logistic 迴歸、odds ratio、完全分離、事件數

- Hosmer, D. W., Lemeshow, S., & Sturdivant, R. X. (2013). *Applied Logistic Regression* (3rd ed.). Wiley. ISBN 9780470582473 `[@hosmer2013]`
- Zhang, J., & Yu, K. F. (1998). What's the relative risk? A method of correcting the odds ratio in cohort studies of common outcomes. *JAMA*, 280(19), 1690–1691. PMID: 9832001 `[@zhang1998]`
- Albert, A., & Anderson, J. A. (1984). On the existence of maximum likelihood estimates in logistic regression models. *Biometrika*, 71(1), 1–10. `[@albert1984]`
- Firth, D. (1993). Bias reduction of maximum likelihood estimates. *Biometrika*, 80, 27–38. `[@firth1993]`
- Heinze, G., & Schemper, M. (2002). A solution to the problem of separation in logistic regression. *Statistics in Medicine*, 21, 2409–2419. `[@heinze2002]`
- Peduzzi, P., Concato, J., Kemper, E., Holford, T. R., & Feinstein, A. R. (1996). A simulation study of the number of events per variable in logistic regression analysis. *Journal of Clinical Epidemiology*, 49, 1373–1379. https://doi.org/10.1016/S0895-4356(96)00236-3 `[@peduzzi1996]`
- van Smeden, M., de Groot, J. A. H., Moons, K. G. M., Collins, G. S., Altman, D. G., Eijkemans, M. J. C., & Reitsma, J. B. (2016). No rationale for 1 variable per 10 events criterion for binary logistic regression analysis. *BMC Medical Research Methodology*, 16, 163. https://doi.org/10.1186/s12874-016-0267-3 `[@vansmeden2016]`

  用於：「odds ratio 不是風險比」（Zhang & Yu）、完全分離的辨識（Albert & Anderson）與 Firth 懲罰迴歸的說明（Firth；Heinze & Schemper），以及「每個參數 10–20 筆只是經驗法則、不是准入標準」（Peduzzi 等提出 EPV 經驗法則；van Smeden 等指出此規則缺乏充分依據）。

## 3. cloglog 連結與離散時間事件

- Prentice, R. L., & Gloeckler, L. A. (1978). Regression analysis of grouped survival data with application to breast cancer data. *Biometrics*, 34, 57–67. https://doi.org/10.2307/2529588 `[@prentice1978]`

  用於：`family-link-selection.md` 中「cloglog 適合離散時間事件史分析」的依據（分組的比例風險模型對應 complementary log-log 連結）。

## 4. 計數資料：Poisson、過度離散、quasi-Poisson、負二項

- Wedderburn, R. W. M. (1974). Quasi-likelihood functions, generalized linear models, and the Gauss–Newton method. *Biometrika*, 61(3), 439–447. https://doi.org/10.1093/biomet/61.3.439 `[@wedderburn1974]`
- Cameron, A. C., & Trivedi, P. K. (1990). Regression-based tests for overdispersion in the Poisson model. *Journal of Econometrics*, 46(3), 347–364. `[@cameron1990]`
- Lawless, J. F. (1987). Negative binomial and mixed Poisson regression. *The Canadian Journal of Statistics*, 15(3), 209–225. `[@lawless1987]`
- Ver Hoef, J. M., & Boveng, P. L. (2007). Quasi-Poisson vs. negative binomial regression: How should we model overdispersed count data? *Ecology*, 88(11), 2766–2772. `[@verhoef2007]`
- Venables, W. N., & Ripley, B. D. (2002). *Modern Applied Statistics with S* (4th ed.). Springer. ISBN 0-387-95457-0 `[@venables2002]`
- Gelman, A., & Hill, J. (2007). *Data Analysis Using Regression and Multilevel/Hierarchical Models*. Cambridge University Press. https://doi.org/10.1017/CBO9780511790942 `[@gelman2007]`
- Fisher, R. A. (1950). The significance of deviations from expectation in a Poisson series. *Biometrics*, 6, 17–24. `[@fisher1950]`

  用於：quasipoisson／quasibinomial 的準概似（Wedderburn）、過度離散檢定（Cameron & Trivedi）、負二項迴歸與 `MASS::glm.nb()`（Lawless；Venables & Ripley）、quasi-Poisson 與負二項變異結構不同、不是等價替代（Ver Hoef & Boveng）、`performance::check_overdispersion()` 對 Poisson 模型的檢定做法（套件說明文件註明依據 Gelman & Hill 第 115 頁）、EDA 中以變異數／平均數比判斷 Poisson 離散程度的離散指數（Fisher）。Gelman & Hill 的出版社頁面列出版日期為 2006 年 12 月、書中註明 2007 年首版，`performance` 套件引用為 2007。

## 5. 零膨脹與門檻模型（只做概念說明）

- Lambert, D. (1992). Zero-inflated Poisson regression, with an application to defects in manufacturing. *Technometrics*, 34(1), 1–14. `[@lambert1992]`
- Mullahy, J. (1986). Specification and testing of some modified count data models. *Journal of Econometrics*, 33(3), 341–365. https://doi.org/10.1016/0304-4076(86)90002-3 `[@mullahy1986]`

  用於：`diagnostics-checklist.md` 第 6 節零膨脹模型（Lambert）與門檻模型（Mullahy）的定義與區別。

## 6. 比例資料與相關觀測（本 skill 不支援，只在告知限制時引用）

- Ferrari, S., & Cribari-Neto, F. (2004). Beta regression for modelling rates and proportions. *Journal of Applied Statistics*, 31(7), 799–815. https://doi.org/10.1080/0266476042000214501 `[@ferrari2004]`
- Liang, K.-Y., & Zeger, S. L. (1986). Longitudinal data analysis using generalized linear models. *Biometrika*, 73(1), 13–22. `[@liang1986]`
- Breslow, N. E., & Clayton, D. G. (1993). Approximate inference in generalized linear mixed models. *Journal of the American Statistical Association*, 88, 9–25. https://doi.org/10.2307/2290687 `[@breslow1993]`

  用於：連續比例資料應考慮 beta regression（Ferrari & Cribari-Neto）；相關觀測需要 GEE（Liang & Zeger）或 GLMM（Breslow & Clayton）。

## 7. 殘差與診斷圖

- Pierce, D. A., & Schafer, D. W. (1986). Residuals in generalized linear models. *Journal of the American Statistical Association*, 81, 977–986. `[@pierce1986]`
- Cook, R. D., & Weisberg, S. (1982). *Residuals and Influence in Regression*. Chapman and Hall. ISBN 0-412-24280-0. https://hdl.handle.net/11299/37076 `[@cook1982]`
- Dunn, P. K., & Smyth, G. K. (1996). Randomized quantile residuals. *Journal of Computational and Graphical Statistics*, 5(3), 236–244. https://doi.org/10.1080/10618600.1996.10474708 `[@dunn1996]`
- Hartig, F. DHARMa: Residual Diagnostics for Hierarchical (Multi-Level / Mixed) Regression Models. R package. https://doi.org/10.32614/CRAN.package.DHARMa （年份與版本以 `citation("DHARMa")` 為準）

  用於：Pearson 與 deviance 殘差（Pierce & Schafer）、`plot(model)` 中的槓桿值（Cook & Weisberg）、DHARMa 模擬殘差的原理（Dunn & Smyth）與套件本身（Hartig）。只有實際用了 DHARMa 才引用後兩筆。

## 8. 共線性

- Fox, J., & Monette, G. (1992). Generalized collinearity diagnostics. *Journal of the American Statistical Association*, 87(417), 178–183. https://www.jstor.org/stable/2290467 `[@fox1992]`
- Fox, J., & Weisberg, S. (2019). *An R Companion to Applied Regression* (3rd ed.). Sage. https://www.john-fox.ca/Companion/ `[@fox2019]`

  用於：GVIF 與 `GVIF^(1/(2*Df))`（Fox & Monette）、`car::vif()` 與 `car::Anova()`（Fox & Weisberg，為 `car` 套件指定的引用）。

## 9. 模型比較：概似比檢定、AIC、BIC

- Wilks, S. S. (1938). The large-sample distribution of the likelihood ratio for testing composite hypotheses. *The Annals of Mathematical Statistics*, 9, 60–62. `[@wilks1938]`
- Akaike, H. (1974). A new look at the statistical model identification. *IEEE Transactions on Automatic Control*, 19(6), 716–723. https://doi.org/10.1109/TAC.1974.1100705 `[@akaike1974]`
- Schwarz, G. (1978). Estimating the dimension of a model. *The Annals of Statistics*, 6(2), 461–464. https://doi.org/10.1214/aos/1176344136 `[@schwarz1978]`
- Burnham, K. P., & Anderson, D. R. (2002). *Model Selection and Multimodel Inference: A Practical Information-Theoretic Approach* (2nd ed.). Springer. https://doi.org/10.1007/b97636 `[@burnham2002]`

  用於：巢狀模型的 LRT（Wilks）、AIC（Akaike）、BIC（Schwarz）、「AIC 差距小於 2 時視為支持程度相近」的經驗判斷（Burnham & Anderson）。

## 10. pseudo-R²

- Tjur, T. (2009). Coefficients of determination in logistic regression models—A new proposal: The coefficient of discrimination. *The American Statistician*, 63(4), 366–372. https://doi.org/10.1198/tast.2009.08210 `[@tjur2009]`
- Nagelkerke, N. J. D. (1991). A note on a general definition of the coefficient of determination. *Biometrika*, 78, 691–692. `[@nagelkerke1991]`

  用於：`performance::r2()` 對 logistic 模型回傳 Tjur R²，對其他 GLM 回傳 Nagelkerke R²（依套件說明文件）。報告時依實際回傳的類型只引用其中一筆。

## 11. 類別變數的估計邊際平均與多重比較

- Searle, S. R., Speed, F. M., & Milliken, G. A. (1980). Population marginal means in the linear model: An alternative to least squares means. *The American Statistician*, 34(4), 216–221. https://doi.org/10.1080/00031305.1980.10483031 `[@searle1980]`
- Lenth, R. V., & Piaskowski, J. emmeans: Estimated Marginal Means, aka Least-Squares Means. R package. https://doi.org/10.32614/CRAN.package.emmeans （年份、版本與作者名單以 `citation("emmeans")` 為準）
- Hsu, J. C. (1996). *Multiple Comparisons: Theory and Methods*. Chapman & Hall. ISBN 9780412982811 `[@hsu1996]`
- Dunnett, C. W. (1955). A multiple comparison procedure for comparing several treatments with a control. *Journal of the American Statistical Association*, 50, 1096–1121. `[@dunnett1955]`

  用於：`emmeans` 的估計邊際平均（Searle 等；Lenth 套件）、Tukey 兩兩比較校正（Hsu 的專書涵蓋 Tukey 法）、只和對照組比較時的 Dunnett 法（Dunnett）。

## 12. 結果解讀：不顯著不等於相同、等效性

- Altman, D. G., & Bland, J. M. (1995). Statistics notes: Absence of evidence is not evidence of absence. *BMJ*, 311(7003), 485. https://doi.org/10.1136/bmj.311.7003.485 `[@altman1995]`
- Schuirmann, D. J. (1987). A comparison of the two one-sided tests procedure and the power approach for assessing the equivalence of average bioavailability. *Journal of Pharmacokinetics and Biopharmaceutics*, 15, 657–680. https://doi.org/10.1007/BF01068419 `[@schuirmann1987]`

  用於：「信賴區間包含 1 只能說資料無法區分」（Altman & Bland）、「宣稱等效需要事先設定界線並做等效性檢定」（Schuirmann 的 TOST）。

## 13. 缺失值

- Rubin, D. B. (1976). Inference and missing data. *Biometrika*, 63(3), 581–592. `[@rubin1976]`
- Little, R. J. A., & Rubin, D. B. (2019). *Statistical Analysis with Missing Data* (3rd ed.). Wiley. ISBN 9780470526798 `[@little2019]`

  用於：缺失機制、完全個案分析可能的偏誤、插補要考慮缺失機制。

## 14. 樣本外預測評估（只在使用者要求交叉驗證時引用）

- Hastie, T., Tibshirani, R., & Friedman, J. (2009). *The Elements of Statistical Learning: Data Mining, Inference, and Prediction* (2nd ed.). Springer. ISBN 9780387848570 `[@hastie2009]`
- Roberts, D. R., Bahn, V., Ciuti, S., Boyce, M. S., Elith, J., Guillera-Arroita, G., Hauenstein, S., Lahoz-Monfort, J. J., Schroeder, B., Thuiller, W., Warton, D. I., Wintle, B. A., Hartig, F., & Dormann, C. F. (2017). Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure. *Ecography*, 40, 913–929. `[@roberts2017]`
- Hanley, J. A., & McNeil, B. J. (1982). The meaning and use of the area under a receiver operating characteristic (ROC) curve. *Radiology*, 143(1), 29–36. `[@hanley1982]`
- Brier, G. W. (1950). Verification of forecasts expressed in terms of probability. *Monthly Weather Review*, 78(1), 1–3. `[@brier1950]`
- Van Calster, B., McLernon, D. J., van Smeden, M., Wynants, L., Steyerberg, E. W., & Topic Group 'Evaluating diagnostic tests and prediction models' of the STRATOS initiative. (2019). Calibration: The Achilles heel of predictive analytics. *BMC Medicine*, 17(1), 230. https://doi.org/10.1186/s12916-019-1466-7 `[@vancalster2019]`

  用於：k 折交叉驗證（Hastie 等）、有時間或群聚結構時要依結構切分（Roberts 等）、AUC（Hanley & McNeil）、Brier score（Brier）、校準不能被 AUC 等指標取代（Van Calster 等）。

## 15. 軟體

- R Core Team. R: A Language and Environment for Statistical Computing. R Foundation for Statistical Computing, Vienna, Austria. https://www.R-project.org/ （年份以 `citation()` 為準）
- Lüdecke, D., Ben-Shachar, M. S., Patil, I., Waggoner, P., & Makowski, D. (2021). performance: An R package for assessment, comparison and testing of statistical models. *Journal of Open Source Software*, 6(60), 3139. https://doi.org/10.21105/joss.03139 `[@ludecke2021]`
- Wickham, H. (2016). *ggplot2: Elegant Graphics for Data Analysis*. Springer-Verlag New York. ISBN 978-3-319-24277-4 `[@wickham2016]`
- `MASS`：引用 Venables & Ripley (2002)（第 4 節）；`car`：引用 Fox & Weisberg (2019)（第 8 節）。
- 其他套件（`dplyr`、`broom`、`knitr`、`rmarkdown`）只在報告需要時，以 `citation("套件名")` 實際輸出的內容引用，不要自行撰寫。
