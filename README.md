# 🏏 Cricket Player Segmentation using K-Means Clustering

## 📖 Project Overview
In the modern era of franchise cricket (IPL, Big Bash, etc.), data-driven player selection is critical. This project utilizes **Unsupervised Machine Learning** to categorize international cricket players into distinct performance-based segments.

By analyzing historical batting statistics, we aim to uncover hidden patterns that group players not just by "good" or "bad," but by **style** and **role** (e.g., the difference between a consistent anchor and an explosive finisher). The core algorithm used is **K-Means Clustering**.

## 📊 Dataset Description
The dataset (`cricket clean.csv`) consists of batting performance metrics for top international cricket players. The features analyzed include:

| Feature | Description | Relevance to Analysis |
| :--- | :--- | :--- |
| **Mat** | Matches Played | Indicates experience and longevity. |
| **Inns** | Innings Batted | A more accurate measure of batting opportunities than Matches. |
| **Runs** | Total Runs | The primary measure of a batter's contribution. |
| **HS** | Highest Score | Indicates peak performance capability. (Cleaned to remove `*` for "Not Out"). |
| **Ave** | Batting Average | A measure of consistency and reliability. |
| **SR** | Strike Rate | A measure of scoring speed/aggression. |
| **100** | Centuries Scored | Indicator of ability to play match-winning innings. |
| **50** | Half-Centuries | Indicator of consistency. |

## ⚙️ Methodology & Workflow

### 1. Data Preprocessing
Raw real-world data is rarely ready for modeling.
* **Cleaning:** The `HS` (Highest Score) column contained numerical values with an asterisk (e.g., `200*`) indicating a "Not Out" innings. These were treated as strings by Python. We stripped the special characters and converted the column to numeric integers to allow mathematical operations.
* **Feature Selection:** We dropped categorical identifiers like `Player` Name for the modeling phase, as K-Means requires numerical input.

### 2. Feature Scaling (Crucial Step)
K-Means calculates clusters based on the Euclidean distance between data points.
* **The Problem:** Variables like `Runs` (range: 0-18,000) have a much larger magnitude than `Average` (range: 0-55). Without scaling, the algorithm would be biased toward `Runs`, treating it as the only important feature.
* **The Solution:** We applied **StandardScaler** to transform all features to have a mean of 0 and a standard deviation of 1 ($\mu=0, \sigma=1$). This ensures every metric contributes equally to the analysis.

### 3. Finding the Optimal Number of Clusters ($k$)
We didn't guess the number of player types; we derived it mathematically:
* **Elbow Method (Inertia):** We plotted the Sum of Squared Distances (SSD) for $k=2$ to $k=10$. The curve flattened significantly at **$k=4$**, forming an "elbow," suggesting that adding more clusters beyond 4 yields diminishing returns.
* **Silhouette Score:** We validated $k=4$ using Silhouette Analysis, which measures how well-separated the clusters are.

### 4. Cluster Profiling
After training the K-Means model with 4 clusters, we analyzed the **mean values** of each cluster to derive meaningful labels.

---

## 🔍 Cluster Analysis & Strategic Insights

We identified 4 distinct player segments. Here is the breakdown of their characteristics and their potential value in a team/auction setting:

### 🥉 Cluster 0: The Consistent Performers
* **Traits:** Solid Batting Average, decent Run tally, and respectable Strike Rate.
* **Role:** These are the backbone of a batting lineup. They provide stability and reliability in the middle order.
* **Strategic Value:** Good value-for-money picks in auctions; reliable but perhaps not "match-winners" on their own.

### 🥇 Cluster 1: The Legends (Elite Tier)
* **Traits:** Highest Runs, Highest Average, High Centuries, and massive Match experience.
* **Examples:** Sachin Tendulkar, Ricky Ponting, Jacques Kallis.
* **Role:** The "All-Time Greats." They excel in every metric.
* **Strategic Value:** Marquee players. They bring brand value, leadership, and guaranteed performance.

### 🥈 Cluster 2: Aggressive Hitters / Impact Players
* **Traits:** Exceptionally high **Strike Rate**, but moderate Average and lower total Runs compared to Legends.
* **Examples:** Shahid Afridi, Virender Sehwag.
* **Role:** Openers or Finishers whose job is to accelerate scoring, even at the risk of getting out.
* **Strategic Value:** Critical for T20 formats where Strike Rate is often more valuable than Average.

### 🪵 Cluster 3: The Lower Order / Developing Players
* **Traits:** Lowest Average and Runs. High variance in Strike Rate.
* **Role:** Tail-enders (bowlers), wicket-keepers who bat low, or players early in their careers.
* **Strategic Value:** Unless they are specialist bowlers, these players have the lowest batting impact.

---

## 📈 Visualizations Used

1.  **3D Cluster Map (Plotly):**
    * *Axes:* Runs (X), Average (Y), Strike Rate (Z).
    * *Insight:* Shows a clear physical separation between the "Legends" (top right corner) and the "Lower Order" (bottom left), with "Aggressive Hitters" branching out along the Strike Rate axis.
2.  **Box Plots:**
    * Used to detect outliers and visualize the spread of data within each cluster (e.g., verifying that Cluster 2 indeed has a higher median Strike Rate than Cluster 0).

## 💻 Installation & Execution

### Prerequisites
Ensure you have Python installed along with the following libraries:
* `pandas`
* `numpy`
* `matplotlib`
* `seaborn`
* `plotly`
* `scikit-learn`

### Steps
1.  **Clone the Repo:**
    ```bash
    git clone [https://github.com/your-username/cricket-player-clustering.git](https://github.com/your-username/cricket-player-clustering.git)
    ```
2.  **Install Requirements:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Analysis:**
    Open the Jupyter Notebook and execute the cells sequentially:
    ```bash
    jupyter notebook Kmeans.ipynb
    ```

## 🔮 Future Scope
* **Hierarchical Clustering:** To visualize the player hierarchy using Dendrograms.
* **PCA (Principal Component Analysis):** To reduce dimensionality and visualize the clusters in 2D space more effectively.
* **T20 Specific Analysis:** Incorporating metrics like "Balls per Boundary" or "Dot Ball %" for modern format analysis.
