
## Buffer
	$^{[1]}$
	(hereinafter referred to as " ")
	(as shown in the \textbf{Figure } )
	(see \textbf{Attachment}).
	 
	{How do we approximate the whole course of ?}
	{How do we define the optimal configuration?}
	{The local optimization and the overall optimization}
	{The differences in weights and sizes of}
	{What if there is no data available?}
	
		\The utility function 效用函数是

		\item The overall optimization:
		\item The local optimization:
		\item The optimal number of    

		\item The cost of       :
		\item The loss of       :
		\item The weight of each aspect:
		\item Compromise:

		1) From the perspective of      :\par
		2) From the perspective of the      :\par
		3) Compromise:
		4) 
	
	1) The overall optimization and the local optimization
	2) 
	3) The solution of the integer programming:
	4) Results:

		\item Local optimization and overall optimization:
		\item Sensitivity: The result is quite sensitive to the change of the three parameters
		\item
		\item Trend:
		\item Comparison:

		\item[Strength:] The Improved Model aims to make up for the neglect of         . The result seems to declare that this model is more reasonable than the Basic Model and much more effective than the existing design.
		\item[Weakness:] Thus the model is still an approximate on a large scale. This has doomed to limit the applications of it.

	{Applications of our models}
	{Methods used in our models}
	{The limitations of queuing theory}
	{Conclusions of the problem}
	{Strength and Weakness}

	Feature extraction of optical information data based on polynomial fitting
	基于多项式拟合的光信息数据特征提取
	
	1) The solution of the integer programming:


总体而言，研究新能源汽车问题旨在实现对环境、气候、能源和经济的可持续性发展
## Summary

如今，中国新能源汽车的发展是一个至关重要的话题，因为它在实现中国碳达峰目标和促进世界新能源产业发展方面发挥着重要作用。然而，中国新能源汽车产业的发展受到国内外多种因素的影响。我们将回答以下问题，并建立一个模型来更好地处理相关问题。
Nowadays, the development of new energy vehicles in China is a crucial topic, given its significant role in achieving carbon peak goals within China and promoting the growth of the world's new energy industry. However, the development of China's new energy vehicle industry is influenced by various domestic and foreign factors. We will answer the following questions and establish a model to better handle related issues.
关于第一个问题，我们根据[钻石理论]选择了主要因素。根据选定的资源条件、政策条件和市场条件，我们收集了国内新能源汽车的数据.并选取了一个可以直接评价行业发展的主要指标。在建立模型之前，我们对其进行了[正态性测试]，发现有太多指标没有通过测试。因此，我们进行了[斯皮尔曼相关性分析]。这些具有代表性的指标可以通过[主成分分析]分解为两个实用指标，并建立[二元回归综合评价模型]来评价其对新能源汽车发展的影响。
Concerning the first issue, we selected the main factors based on the [diamond theory]. According to the selected resource conditions, policy conditions, and market conditions, we then collected domestic data on new energy vehicles. And selected a main indicator that can directly evaluate the development of the industry. Before building the model, We conducted a [normality test] on it and found that there were too many indicators that did not pass the test. Therefore, we conducted a [Spearman correlation analysis] .These representative indicators can be reduced into two practical indicators through [principal component analysis], and establish a [binary regression comprehensive evaluation model] to evaluate their influence on the development of new energy vehicles.

关于第二个问题，我们首先绘制了5个指标的散点图，以观察每个指标随时间的增长趋势。基于[产业发展周期理论]，我们考虑使用[S曲线模型]来预测变化符合[逻辑函数]特征的指标。我们使用[I线性模型]和[L对数模型]来预测相对简单的指标。我们使用[ARIMA时间序列模型]来预测季节变化。最后，我们结合第一个问题的[基于主成分分析的二元回归综合评价模型]来预测未来十年新能源汽车销量的发展。
Concerning the second issue, we first draw scatter plots of 5 indicators to observe the growth trend of each indicator over time. Based on the [Industry Lifecycle Theory], We consider using the [S-curve Model] to predict indicators whose changes conform to the characteristics of the [Logistic function]. We use the [I-linear model] and the [L-logarithmic model] to predict relatively simple indicators. We use the [ARIMA Model] to predict seasonal changes. Finally, we combine the previous [Binary Regression Comprehensive Evaluation Model] based on [Principal Component Analysis] to predict the development of new energy vehicle sales in the next decade.


2.  锂开采量：（94年-22年数据）【美国地质局】

存在一定的周期变化，考虑用时间序列

1.   锂电池续航 用直线拟合
2.  中国总汽车销量用对数拟合


## Contents

[目录]
## Introduction

#### Background
随着"碳达峰、碳中和”逐渐成为现代化建设的核心议题,中国在十四五规划提出单位国内生产总值二氧化碳排放降低18%的目标，落实 2030 年应对气候变化国家自主贡献目标，锚定努力争取 2060 年前实现碳中和。以能源革命、产业结构调整、新能源汽车推广为主要抓手推进节能减排,节能减排是“碳中和”政策重点。
根据国际能源署二氧化碳2022年报，与全球交通运输业排放量的增长形成对比，中国交通运输业的排放量于2022年下降3.1%。根据按区域和部门划分的二氧化碳排放量变化图中可以看到，世界交通运输业的二氧化碳排放量占比大，这也意味着我们不能忽视汽车产业所带来的影响。报告中还提出，电动汽车产业作为新兴产业，防止了柴油和汽油汽车的进一步排放，这也说明了电动汽车产业的重要性。
![[Pasted image 20231124212954.png]]
[Change in CO2 emissions by region and by sector, 2021-2022]

Since the advent of electric vehicles powered by renewable energy, their advantages over traditional vehicles, such as low pollution, energy efficiency, and the ability to regulate peak power, have made them widely popular worldwide.
自从可再生能源驱动的电动汽车问世以来，其相对于传统汽车的优势，如低污染、能源效率和调节峰值功率的能力，使其在全球范围内广受欢迎。
According to a research report on new energy vehicles by the International Energy Agency( hereinafter referred to as " "), three major markets dominate global sales: the United States, Europe, and China. It is noteworthy that China has once again taken the lead, representing approximately 60% of global electric vehicle sales. At present, over half of the electric vehicles on roads across the world are being used in China.
根据国际能源署(后文简称为IEA)关于新能源汽车的研究报告，三个主要市场主导着全球销量：美国、欧洲和中国。值得注意的是，中国再次占据领先地位，约占全球电动汽车销量的60%。目前，全球道路上超过一半的电动汽车在中国使用。
![[Pasted image 20231124163735.png]]
[Global electric car stock in selected regions, 2010-2022]

随着人们对新能源的日益关注，世界各国政府都注意到了新能源汽车行业的发展，并制定了支持新能源汽车的政策法规。这些法规的实施将有助于新能源汽车的广泛采用。其中，中国的政策尤其稳定（见附件），推动了新能源汽车行业的快速发展。
随着电池技术、充电基础设施等相关方面的进步，中国在该领域的技术正在不断发展。与此同时，电动汽车行业的快速增长对传统汽车行业产生了重大影响。传统汽车制造商正在增加对电动汽车行业的投资，以应对激烈的行业竞争，促进供应链调整和技术创新。
全球汽车制造商对电动汽车投资的增长加剧了市场竞争。一些国家对中国新能源汽车行业实施了抵制政策，试图在同一领域获得竞争优势。
但除此之外，我们必须承认，新能源汽车将成为汽车行业的一个积极趋势，并有助于缓解汽车作为一种交通方式对生态的有害影响。
With people's increasing attention to new energy, governments around the world have noticed the development of the new energy vehicle industry and formulated policies and regulations to support new energy vehicles. The implementation of these regulations will contribute to the widespread adoption of new energy vehicles. Among them, China's policies are particularly stable (see attachment), which have driven the rapid development of the new energy vehicle industry.
With the progress of battery technology, charging infrastructure and other related aspects, China's technology in this field is constantly developing. At the same time, the rapid growth of the electric vehicle industry has had a significant impact on the traditional automotive industry. Traditional car manufacturers are increasing their investment in the electric vehicle industry to cope with fierce industry competition, promote supply chain adjustment, and technological innovation.
The growth of global automobile manufacturers' investment in electric vehicles has intensified market competition. Some countries have implemented boycott policies against China's new energy vehicle industry in an attempt to gain a competitive advantage in the same field.
But aside from the above, we must acknowledge that new energy vehicles will become a positive trend in the automotive industry and help alleviate the harmful impact of cars as a mode of transportation on the ecology.
		
		
2022年12月31日，持续13年的新能源汽车购置补贴政策终止，
2010 年中期售出的新能源汽车所搭配的电池将迎来第一波退役潮

#### Restatement of the Problem

Question 1: Analyze the main factors that affect the development of new energy electric vehicles in China, and establish a mathematical model to elucidate the impact of these factors on the growth of China's new energy electric vehicle industry.
问题1：分析影响中国新能源电动汽车发展的主要因素，建立数学模型，用于阐明这些因素对中国新能源电动车产业增长的影响。

Question 2: Collect data on the development of China's new energy electric vehicle industry, establish a mathematical model to depict and forecast the development trend of China's new energy electric vehicles over the next decade.
问题2: 收集中国新能源电动汽车产业发展的数据，建立数学模型来描绘和预测未来十年中国新能源电动车的发展趋势。

Question 3: Collect relevant data on the global traditional energy vehicle industry, establish a mathematical model, and analyze the impact of new energy vehicles on the traditional energy vehicle industry.
问题3: 收集全球传统能源汽车行业的相关数据，建立数学模型，分析新能源汽车对传统能源汽车产业的影响。

Question 4: Investigate policies in certain countries that resist the development of new energy electric vehicles in China. Establish mathematical models to analyze the impact of these policies on the development of new energy electric vehicles in China.
问题4: 调查某些国家的抵制中国新能源电动汽车发展的政策。建立数学模型分析这些政策对中国新能源电动汽车发展的影响.

Question 5: Collect relevant data and establish mathematical models to analyze the impact of urban electrification on the ecological environment. Provide the calculation results of the model when a city with a population of 1 million is considered as a condition.
问题5: 收集相关数据，建立数学模型，分析城市电气化对生态环境的影响。提供将100万人口的城市作为一个条件时模型的计算结果。

Question 6: Draft an open letter to citizens based on the conclusion of question 5, popularize the advantages of new energy electric vehicles and emphasizing the contribution of the electric vehicle industry in various countries around the world.
问题6: 根据问题5的结论起草一封致公民的公开信，普及新能源电动汽车的优势，并强调电动汽车行业在世界各国的贡献。

#### Our work

针对上述问题，本文展开了以下工作
针对问题1，
[流程图]


## Model Establishment and Solution for Problem 1

#### Terminology and Definitions

钻石理论，又称为菱形理论及国家竞争优势理论,（Michael Porter's National Diamond）[1]  [Sledge, S."Does Porter's diamond hold in the global automotive industry”, Advances in Competitiveness Research,Vol.1] 可以分为以下几点:

- 因素条件（Factor Conditions）： 国家内部的生产要素，如人力资源、土地、资本和基础设施。
- 需求条件（Demand Conditions）： 国内市场对产品的需求水平和品质要求。
- 相关与支持性行业（Related and Supporting Industries）：与产业相关的附属产业和支持产业，共同推动产业的发展。
- 公司战略、结构与竞争力（Firm Strategy, Structure, and Rivalry）：公司在国内的竞争情况以及其战略和组织结构。
- **国家因素（Government）：** 政府对产业的支持和干预，包括法规、政策和资金。

The Diamond Theory, also known as the National Competitive Advantage Theory, can be divided into the following points:
- Factor Conditions: The production factors within a country, such as human resources, land, capital, and infrastructure.
- Demand Conditions: The level of demand and quality requirements for products in the domestic market.
- Related and Supporting Industries: Affiliated and supporting industries related to an industry, jointly promoting the development of the industry.
- Firm Strategy, Structure, and Rivalry: The competitive situation of a company in China, as well as its strategic and organizational structure.
- Government factors: The government's support and intervention in industries, including regulations, policies, and funding.
#### Hypothesis and Justifications
-
-
-
-
#### Notation

The signs and definitions are mostly generated from queuing theory.
#### Collection and Analysis 

##### Factor Selection

钻石理论，又称为菱形理论及国家竞争优势理论, 是由经济学家迈克尔·波特（Michael Porter）提出的一种解释国家产业竞争优势的框架, 框架内由Factor Conditions,Demand Conditions, Related and Supporting Industries,Firm and Government 这五个因素之间的相互作用，使得它们共同塑造了一个国家的产业环境.
回到我们的问题,我们将基于钻石理论分析可以得到三个影响新能源汽车发展的条件[2]  [新能源汽车发展影响因素的调研及分析[D].朱恺.华南理工大学],以此作为指标进行模型的建立
- Resource Condition: 知识资源、资本资源、自然资源、人力资源都属于生产要素, 而相关产业的成熟发展为新能源汽车提供了丰富的技术资源, 两者可以视为资源条件. 
- Policy Condition: 政府在新能源汽车领域的政策支持为我国创造了广泛的发展机遇，
- Market Condition: 市场因素直接影响需求条件，同时也涉及到结构和竞争、企业战略等方面。因此，这些方面可以综合视为市场条件。

![[Pasted image 20231127010429.png]]


Analysis and selection of the main factors that can influence the development of new energy vehicles on the basis of the diamond model.
图一: 基于钻石模型分析并选取能够影响新能源汽车发展的主要因素

##### Data Collection
通过因素的种类划分，我们在钻石模型的基础上进行了数据的收集: [问题一数据-附件] (该数据均对中国统计)
- 资源条件
	- 新能源汽车最基本的驱动产品就是电池,我们从电池的性能上入手，收集汽车随电池技术发展所增加的里程数据(后文简称为[Battery Endurance])，这些数据属于技术资源，
	- 我们又从电池的原材料上入手,收集锂矿开采量的数据(后文简称为[Li yield]),这些数据属于自然资源，
	- 而新能源汽车的互补产品中，充电桩产业的发展可以视为相关支持产业，我们收集了充电桩数量的数据(后文简称为[Charing Point])，视为技术资源(也就是知识资源),
- 政策条件
	- 由于政策的多变性以及侧重点不同，我们暂时不对其进行量化，但这并不意味着我们会忽视政策所带来的影响。
- 市场条件: 
	- 我们收集了'新能源汽车的生产总量'数据(后文简称为[EV yield]),新能源汽车的产量一定程度上映射着市场规模，这些生产规模是直接被新能源汽车企业的战略决定的，也是被市场结构和竞争影响的。根据供求的调节机制，新能源汽车的供给能够直接体现了新能源汽车的需求，市场需求越大,供给也就会越大,因此'新能源汽车的产量'也可以作为评估市场的指标
	- 除此之外,我们还收集了'新能源汽车的销售总量'的数据(后文简称为[EV sale]),在商业贸易当中，交易作为最重要的一个环节,我们决定选取这一重要指标作为直接衡量新能源产业发展的唯一指标, 该指标后续也将区分于其他指标,抽象成一个衡量标准
	- 同时为了更好的评估市场，我们还收集了'中国汽车销售总量'的数据(后文简称为[V sale]) ，可以了解新能源汽车在整个汽车市场中所占的份额, 计算出了'新能源电车在市场的占有率'这一指标(后文简称为市场份额),公式如下[公式1],我们可以初步以此评估新能源汽车的竞争力或者在市场上的渗透程度。但是该指标不参与到后续的模型构建.
$$\frac{EV sale}{V sale}$$
[市场份额时间曲线]
##### Normality Test

我们对收集到的数据进行初步的检验，尝试理解数据特征，识别出数据异常，我们使用了SPSS2019对其进行了统计描述和正态分布检验。
通过计算均值、中位数、标准差等统计量，我们可以得知数据的集中趋势、离散程度和分布形状，有助于把握数据的基本情况。介于样本数据较少，我们采用了Kolmogorov-Smirnov（KS）检验以及Shapiro-Wilk检验。KS检验对于样本容量较小的情况表现较好，但在样本容量较大时可能会变得过于敏感。对于正态分布检验，Shapiro-Wilk检验在样本容量不是很大时可能更为适用,对比效果之后，我们选择Shapiro-Wilk检验结果进行分析。我们发现，根据α=0.05的检验水准，其中‘EV yield’，‘EV sale’，‘EV share’不满足正态性检验，以下是正态分布检验的结果
	We conducted preliminary tests on the collected data, trying to understand its characteristics and identify data anomalies. We used SPSS 2019 to perform statistical descriptions and normal distribution tests on it.
	By calculating statistics such as mean, median, and standard deviation, we can determine the concentration trend, dispersion, and distribution shape of the data, which helps us grasp the basic situation of the data.
	Due to the limited sample data, we used Kolmogorov Smirnov (KS) test and Shapiro Wilk test. The KS test performs well for small sample sizes, but may become overly sensitive when the sample size is large. For the normal distribution test, the Shapiro Wilk test may be more suitable when the sample size is not very large. After comparing the results, we choose the Shapiro Wilk test results for analysis.
	We found that according to α= A testing level of 0.05, where 'EV yield', 'EV sale', and 'EV share' do not meet the normality test, the following are the results of the normality test。

[统计描述(表)&正态分布检验]
值得一提的是，我们发现，正态分布检验的结果图中出现了离群现象(如图所示)，‘EV yield’‘Charing Point’‘EV sale’‘EV share’四个指标都在2022年出现了离群值，通过相关资料考察[2023年起，新能源汽车购置补贴政策终止——补贴退场，新能源汽车如何“续航”？-新华网 (news.cn)] (http://www.news.cn/fortune/2023-02/21/c_1129382110.htm)，中国政府在该年年底终止了新能源汽车购置补贴政策。我们将其认为是“政策的影响”。并找出了中国近些年的政策,详情见附件[政策-附件]
[离群值]

##### Correlation Analysis

斯皮尔曼相关性分析不要求数据是正态分布的，也不要求变量之间的关系是线性的。它主要基于变量的秩次而不是实际的数值，在样本容量较小或数据有序而非连续的情况下，斯皮尔曼相关性分析可能是更可靠的选择，于是我们决定采用斯皮尔曼相关性分析，以下是斯皮尔曼相关性分析的原理：
	Spearman correlation analysis does not require data to be normally distributed, nor does it require the relationship between variables to be linear. It is mainly based on the rank of variables rather than actual values. In cases where the sample size is small or the data is ordered rather than continuous, Spearman correlation analysis may be a more reliable choice, so we decided to use Spearman correlation analysis.

1. **秩次分配：** 对于每一个变量，在数据集中将其值从小到大排列，并为每个值分配一个秩次。秩次是该值在排序列表中的位置。如果有相同的值，则其秩次是这些值的平均秩次。
   
    **Rank Assignment:** For each variable, the values are arranged in ascending order, and ranks are assigned to each value. The rank is the position of the value in the sorted list. If there are ties (equal values), their ranks are the average of those tied values.

1. **计算差异：** 对应每一对数据点，计算两个变量的秩次差异($d$)。即，对于第 $i$ 个数据点 ,它的计算公式为：

	 **Calculate Differences:** For each pair of data points, calculate the difference in ranks( $d$ ). For the $i$-th data point, its calculation formula is:	$$d_i=\text { Rank }_{\text {Variable 1 }}-\text { Rank }_{\text {Variable 2 }}$$
1. **计算斯皮尔曼相关性系数：** 用所有的 $d_i$​ 计算斯皮尔曼相关性系数 $p$，它的计算公式为：
	
	**Calculate Spearman Rank Correlation Coefficient ( $ρ$ ):** Use all $d_i$​ values to compute the Spearman rank correlation coefficient ( $ρ$ ) using the formula:$$\rho=1-\frac{6 \sum d_i^2}{n\left(n^2-1\right)}$$
     where $n$ is the number of data points.
    
1. **显著性检验：** 通过检查斯皮尔曼相关性系数是否显著来判断变量之间的关系。通常使用零假设 $H_0: \rho=0$，并进行假设检验。
   
    **Significance Test:** Assess the relationship between variables by checking whether the Spearman correlation coefficient is statistically significant. Typically, a null hypothesis $(H_0: \rho=0)$ is assumed, and hypothesis testing is performed.

1. **解释系数：** 斯皮尔曼相关性系数的取值范围在 $-1$ 到 $1$ 之间，$-1$ 表示完全的负相关，$1$ 表示完全的正相关，$0$ 表示无相关性
	
	**Interpretation of the Coefficient:** The Spearman correlation coefficient ranges between $-1$ and $1$. A value of $-1$ indicates a perfect negative correlation, $1$ indicates a perfect positive correlation, and $0$ indicates no correlation.

我们使用SPSS2019这些数据进行了斯皮尔曼相关性分析, 通过检验发现,新能源汽车总销量这一重要指标与中国汽车销售总量，充电桩数量，新能源生产规模，锂矿年开采量，新能源电池的续航能力的相关性显著,相关系数如下图所示
	We used the data of SPSS2019 to conduct Spearman correlation analysis. Through inspection, we found that the total sales of new energy vehicles, an important indicator, was significantly correlated with the total sales of Chinese vehicles, the number of charging piles, the scale of new energy production, the annual mining output of lithium mines, and the endurance of new energy batteries. The correlation coefficient is shown in the following figure.
[斯皮尔曼相关性分析]

#### Establishment and Solution 

##### Principal Component Analysis
主成分分析（Principal Component Analysis，PCA）是一种用于降低数据维度的多元统计方法。其主要目标是通过线性变换将高维数据转化为低维数据，同时保留数据中的主要信息。

在模型构建之前,我们将唯一指标也就是新能源汽车总销量先放在一边,因为唯一指标我们将用于衡量新能源产业发展状况,以及用于评估新能源汽车市场竞争力的市场份额这一指标我们也不并参与到模型的构建中,它是由$\frac{EV sale}{V sale}$计算得来的,仅用于初步评估新能源汽车的竞争力

我们希望对剩下的五个指标进行数据的降维处理,于是进行了主成分分析,以下是原理和步骤(注意,实际的实现涉及一些额外的标准化步骤，并考虑了数据的中心化)
	Principal Component Analysis (PCA) is a multivariate statistical method used to reduce data dimensions. Its main goal is to transform high-dimensional data into low dimensional data through linear transformation, while preserving the main information in the data.
	Before constructing the model, we set aside the only indicator, which is the total sales volume of new energy vehicles, because we will use the only indicator to measure the development status of the new energy industry and the market share used to evaluate the competitiveness of the new energy vehicle market. This indicator is not involved in the model construction and is calculated by $\frac{EV sales}{V sales}$, only used for preliminary evaluation of the competitiveness of new energy vehicles
	We hope to perform data dimensionality reduction on the remaining five indicators, so we conducted principal component analysis. The following are the principles and steps (note that the actual implementation involves some additional standardization steps and considers data centralization)
		
1. **协方差矩阵:**$$\Sigma = \frac{1}{n} \sum_{i=1}^{n}(X_i - \bar{X})(X_i - \bar{X})^T$$这里，$X_i$ 代表数据点，$\bar{X}$ 是均值向量，$n$ 是数据点的数量，$\Sigma$ 是协方差矩阵。
2. **特征值分解:**$$\Sigma v = \lambda v $$其中，$\lambda$ 是特征值，$v$ 是相应的特征向量。
3. **选择主成分:**
   主成分是基于最大特征值选择的。第一个主成分对应于最大特征值的特征向量，第二个主成分对应于第二大特征值，依此类推.
2. **解释方差:** Variance Explained by PC
   每个主成分解释的方差可以计算为其特征值与所有特征值之和的比率：$$\text{E}_i = \frac{\lambda_i}{\sum_{j=1}^{p} \lambda_j}$$这里，$p$ 是原始变量的数量。

1. **Covariance Matrix:**
   $$\Sigma = \frac{1}{n} \sum_{i=1}^{n}(X_i - \bar{X})(X_i - \bar{X})^T$$
   Here, $X_i$ represents the data points, $\bar{X}$ is the mean vector, $n$ is the number of data points, and $\Sigma$ is the covariance matrix.

2. **Eigenvalue Decomposition:**
   $$\Sigma v = \lambda v $$
   Where $\lambda$ is the eigenvalue, and $v$ is the corresponding eigenvector.

3. **Selection of Principal Components:**
   Principal components are selected based on the largest eigenvalues. The first principal component corresponds to the eigenvector associated with the largest eigenvalue, the second principal component corresponds to the second-largest eigenvalue, and so on.

4. **Explained Variance:**
   Variance explained by each principal component can be calculated as the ratio of its eigenvalue to the sum of all eigenvalues:
   $$\text{E}_i = \frac{\lambda_i}{\sum_{j=1}^{p} \lambda_j}$$
   Here, $p$ is the number of original variables.

在进行主成分分析时,我们发现前两个主成分的总贡献率已经达到了95.0340,此时基本可以认为这五个指标可以被压缩成y1和y2两个变量,我们也同时能够发现标准化变量的前两个主成分最具有实际意义,根据相关系数矩阵发现第一主成分与'Battery Endurance'最为相关,第二主成分与'V sale'以及'EV yield'最为相关,我们将两个主成分分别解释为"Resource"主成分和"Market"主成分,可以得到若干个主成分方程用于后续的多元线性回归分析,主成分方程如下

[主成分分析法的相关系数矩阵以及贡献率]
$$y_1 = 0.4782 x_1  -0.3183x_2   -0.0105x_3+ 0.3194x_4 +0.7536x_5$$
$$y_2   = 0.4614 x_1  -0.3344 x_2+  0.5908x_3 +  0.2294x_4  -0.5230x_5$$
$$y_3=...$$
$$...$$
##### Binary Linear Regression

通过主成分分析,我们得到了两个主成分,这两个主成分是由多个指标综合计算而成的,我们接下来将$y_1$和$y_2$作为新的变量,设$y_1$和$y_2$的系数分别为$\lambda_1$和$\lambda_2$,而先前考虑的政策条件将抽象成一个偏差项 $\lambda_0$ ,设出一条多元线性回归模型,我们将'EV sale'作为基数据,对设定的模型进行拟合,求解两个系数,以下是一个主成分综合评价模型:

$$z=\lambda_1y_1+\lambda_2y_2+\lambda_0$$


 拟合优度(R-squared)可以衡量模型对因变量变异的解释程度。R-squared的取值范围在0到1之间，越接近1表示模型对数据的拟合越好。通过SPSS2019进行多元线性回归分析后发现效果达到0.912，说明拟合效果好，DW检验达到1.303，模型随机干扰项的自相关度较小，可以认为不存在自相关,该模型能够很好的评估出这些主要因素对新能源汽车发展的影响.


通过P-P图以及直方图得出，回归标准化残差呈现正态分布，回归标准化残差为0.905，说明模型拟合效果很好

通过SPSS拟合结果得出
Y = 7.529 X1 -0.22 X2 -124550.873

但X1，X2对应系数标准误分别为24.53,23.685说明回归系数存在一定的不确定性，可能存在一定的偏差。
 
[多元回归分析图以及拟合效果图]

## Model Establishment and Solution for Problem 2

#### Hypothesis and Justifications

#### Notation

#### Establishment and Solution
##### S-curve Model Based On industry lifecycle theory
[Logistic function - Wikipedia --- 逻辑函数 - 维基百科](https://en.wikipedia.org/wiki/Logistic_function)
产品生命周期理论最早由美国经济学家雷蒙德·弗拉特·斯特尔纳（Raymond Vernon）于1966年提出。[Vernon, Raymond. International Investment and International Trade in the Product Cycle. 1966.]中首次提出了这个概念。斯特尔纳认为，随着时间的推移，创新的产品在国内市场上被引入后，它们会经历一系列的发展阶段，包括在国际市场上的推广和成熟,
而这样一个现象可以通过不同的数学方程来表示, 而Logistics Function这一"S 曲线（Sigmoid Curve）",就是其中一种常见的形式，它有着缓慢开始、迅速增长和逐渐趋于饱和的特点。
[充电桩数量时间曲线] & [新能源汽车产量]
根据产品生命周期理论,我们认为充电桩数量所反映的充电桩产业,以及EV yield所反映的新能源汽车产业,其发展趋势也应该符合生命周期理论,我们决定将这些数据进行非线性拟合,所用到的拟合函数正是S曲线, 其中 $N$ 表示时间累加值,这里代表着年份(), $k$ 表示一个上限值或饱和值，即当 $N$ 变得非常大时，分式的值趋向于 $k_1$ , 而 $a_1$ 控制着分式在 $N$ 较小时的增长速度, $b_1$ 是一个调整因子，控制着指数项 $e^{a_1*N}$ 的影响程度。较大的 $b_1$ 值可能导致趋势更为陡峭。其方程为:
$$S(N)=\frac{k_1}{(1+a_1×e^{-b_1×N})}$$
通过SPSS2019结合实际数据进行拟合，可以得到最优的参数 $k_1$, $a_1$, $b_1$ 的估计值，从而更好地描述和理解数据的变化趋势, 我们再根据模型去预测得到未来十年的数据,以下是拟合结果和预测结果

[logistics曲线非线性拟合结果]

[充电桩以及新能源产量的部分预测数据]

##### linear model and logarithmic model 

[锂电池续航技术曲线]

我们认为锂电池续航的技术在短短十年内并不会有较大的突破,且散点图呈现一个线性趋势,因此我们决定使用直线方程对该数据集进行拟合,其中我们可以直接根据已知的散点图数据快速求解斜率 $a_2$ 以及截距 $b_2$ .易得到方程为:
$$I(N)=a_2×N+b_2$$
通过SPSS2019结合实际数据进行拟合，可以得到最优的参数 $a_2$, $b_2$ 的估计值，从而更好地描述和理解数据的变化趋势, 我们再根据模型去预测得到未来十年的数据,以下是拟合结果和预测结果

[锂电池续航的部分预测数据]

[中国汽车总销量曲线]

x$$L(N)=a_3\log{N}+b_3$$通过SPSS2019结合实际数据进行拟合，可以得到最优的参数 $b$, $c$ 的估计值，从而更好地描述和理解数据的变化趋势, 我们再根据模型去预测得到未来十年的数据,以下是拟合结果和预测结果

[中国汽车总销量的部分预测数据]

##### Nonlinear Auto Regressive Integrated Moving Average Model

[锂矿开采量曲线]

观察锂矿开采量的变化,我们发现其存在一定季节性变化,因此我们决定进行时间序列预测, 在众多时间序列模型中,我们选择二了ARIMA（Autoregressive Integrated Moving Average）,ARIMA模型是一种相对简单但有效的时间序列模型,R部分表示过去值对当前值的影响，MA部分表示过去的误差对当前值的影响，差分部分则表示为了使序列平稳所做的差分次数。而且它不需要太多的参数和复杂的假设，对于许多时间序列问题，ARIMA模型可以提供合理的拟合和预测.

1. **平稳性：** ARIMA模型要求时间序列是平稳的，即它的统计性质不随时间发生显著变化。如果时间序列不是平稳的，需要通过差分操作将其转化为平稳序列。差分的阶数通常用 d 表示。
    
2. **自回归（AR）：** AR部分表示当前值与过去的值之间的关系，即自相关。AR的阶数通常用 p 表示，表示模型中考虑的过去时间点的数量。
    
3. **移动平均（MA）：** MA部分表示当前值与过去的误差（白噪声）之间的关系，即移动平均。MA的阶数通常用 q 表示，表示模型中考虑的过去误差的数量。

ARIMA模型的数学表达式为：
$$ Y_t = \mu + \phi_1 Y_{t-1} + \phi_2 Y_{t-2} + \ldots + \phi_p Y_{t-p} + \theta_1 \varepsilon_{t-1} + \theta_2 \varepsilon_{t-2} + \ldots + \theta_q \varepsilon_{t-q} + \varepsilon_t $$
其中$Y_t$ 是时间序列在时刻 $t$ 的值。 $\mu$ 是常数项。 $(\phi_1, \phi_2, \ldots, \phi_p)$ 是自回归参数。$( \varepsilon_t )$是白噪声误差项。$( \theta_1, \theta_2, \ldots, \theta_q )$ 是移动平均参数。

ARIMA模型的建立通常包括选择合适的 \(p\)、\(d\)、\(q\) 值，拟合模型参数，检验模型残差，最终进行预测。自相关函数（ACF）和偏自相关函数（PACF）的分析通常用于确定 \(p\) 和 \(q\) 的值。以下是ACF和PACF的分析结果

[ACF&PACF分析图]

[ARIMA最终的矿采量预测图]

##### model prediction

以上的S曲线模型,直线模型,对数模型,以及ARIMA模型所预测得到的十年数据,我们将其一一带入第一问的两个主成分方程中,求得未来每一年的$y_1^{'}$和$y_2^{'}$ ,我们再将其一一带入到第一问的双元线性回归方程中,求得未来每一年的$z^{'}$ ,其实就是最终预测的新能源汽车年销量,由于其暂时作为衡量新能源汽车发展程度的唯一指标,因此我们最终通过这个模型预测了未来十年新能源汽车的发展趋势.

[流程图]

[最终预测趋势图]

## Model Establishment and Solution for Problem 3

#### 

#### 

#### 
## Model Establishment and Solution for Problem 4

#### 

#### 

#### 
## Model Establishment and Solution for Problem 5



## Conclusion

根据《巴黎协定》关于“在本世纪下半叶实现温室气体源的人为排放与汇的清除之间的平衡”的要求，中国二氧化碳排放力争于2030年前达到峰值，努力争取2060年前实现碳中和。
## Reference 

[1] Wikipedia, Logistic function, https://en.wikipedia.org/wiki/Logistic_function, (2023,11,25).
[ ] Vernon Raymond, International Investment and International Trade in the Product Cycle[M], 1966.
[ ] Sledge S, Does Porter's diamond hold in the global automotive industry[J], Advances in Competitiveness Research, 2005, Vol.1, PP22-32.
[ ] Kai Zhu, Research and analysis on the influencing factors of the development of new energy vehicles[D], South China University of Technology, 2017,11,27.
[ ] ZiQuan Long, JingMin Chang, etc, A Study on the Impact of Incentive Policies on the Promotion of New Energy Vehicles: An Empirical Analysis Based on the Modified Bass Model[J].  
Science and Technology Management Research, 2016, 36(4):138-144.
[] Xinhuanet, Starting from 2023, the subsidy policy for purchasing new energy vehicles will be terminated - subsidies will be withdrawn. How can new energy vehicles "last"?, http://www.news.cn/fortune/2023-02/21/c_1129382110.htm, (2023,11,25)
[1] Xueyan Wang. Multi-period multi-product supply chain network design under price and quality competition[D]. Huazhong University of Science and Technology,2020.DOI:10.27157/d.cnki.ghzku.2020.005106.
[1] Liu Na. Research on environmental product quality and price strategy considering consumers' environmental awareness and government subsidy [D]. Shandong Normal University,2021.DOI:10.27280/d.cnki.gsdsu.2021.000993.
[1] Liang Mei. Research on environmental impact and life cycle cost of new energy vehicles and traditional fuel vehicles[D]. China University of Petroleum (Beijing),2020.DOI:10.27643/d.cnki.gsybu.2020.001292.
## Appendices

#### 问题一(主成分分析法)

```c
clc;

clear;

% Read data from Excel file

[X, textdata] = xlsread('TableSummary.xlsx');

% Standardize the data

XZ = zscore(X);

% Calculate the covariance matrix

r = corrcoef(XZ);

% Perform PCA analysis

[vec1, lamda, rate] = pcacov(r);

% Cumulative explained variance

contr = cumsum(rate);

% Standardize the principal component loadings

f = repmat(sign(sum(vec1)), size(vec1, 1), 1);

vec2 = vec1 .* f;

% Select the number of principal components

num = 2;

% Calculate the principal component scores

df = XZ * vec2(:, 1:num);

% Calculate the weights of the principal component scores

tf = df * rate(1:num);

% Principal component scores as a percentage

tf_percent = df * rate(1:num) / 100;

% Sort the principal component scores

[stf, ind] = sort(tf, 'descend');

stf = stf';

ind = ind';
```


## Waiting list
 （Sensitive Analysis）
 （Model Evaluation and Further Discussion）