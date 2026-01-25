<div align="center">

# Awesome Matplotlib [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

📊 **Master data visualization for Machine Learning & Deep Learning in record time - no fluff, just the good stuff!**

**Latest for 2026** 🚀

<br>
<br>
</div>

## 🎯 Why This Guide?

You're here to visualize your **ML models, not become a design expert**. This guide covers exactly what you need for:
- Exploratory Data Analysis (EDA)
- Training curves & metrics
- Model evaluation plots
- Feature analysis
- Results presentation

No unnecessary theory. Just practical, copy-paste-ready code. Let's go! 🚀


## 📚 Quick Navigation

1. [Setup & First Plot](#-setup--first-plot)
2. [The Two Interfaces](#-the-two-interfaces-pick-your-style)
3. [Essential Plots for EDA](#-essential-plots-for-eda)
4. [Training Visualizations](#-training-visualizations)
5. [Model Evaluation](#-model-evaluation-plots)
6. [Subplots & Layouts](#-subplots--layouts)
7. [Styling & Themes](#-styling--themes)
8. [Saving Figures](#-saving-figures-for-papers--reports)
9. [Quick Reference](#-quick-reference-cheat-sheet)

---

## 🛠 Setup & First Plot

### Installation

```bash
# Basic
pip install matplotlib

# With extras (recommended)
pip install matplotlib seaborn  # seaborn makes things prettier

# For Jupyter notebooks
pip install ipympl  # Interactive plots
```

### Your First Plot (30 seconds!)

```python
import matplotlib.pyplot as plt
import numpy as np

# Simple line plot
x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('My First Plot!')
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.show()
```

### Jupyter Setup

```python
# At the top of your notebook
%matplotlib inline  # Static plots
# OR
%matplotlib widget  # Interactive plots (with ipympl installed)

import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

# Make plots prettier (optional but recommended)
sns.set_style('whitegrid')
plt.rcParams['figure.figsize'] = (10, 6)
plt.rcParams['figure.dpi'] = 100
```


## 🎨 The Two Interfaces: Pick Your Style

Matplotlib has **two ways** to do things. Here's when to use each:

### 1. Pyplot Interface (Quick & Simple)

**Use for**: Quick plots, Jupyter notebooks, exploration

```python
# Simple and fast
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.title('Quick Plot')
plt.show()
```

### 2. Object-Oriented Interface (Flexible & Powerful)

**Use for**: Multiple plots, customization, production code

```python
# More control
fig, ax = plt.subplots()
ax.plot([1, 2, 3, 4], [1, 4, 9, 16])
ax.set_title('OO Style Plot')
plt.show()
```

**🎯 Pro Tip**: Use OO style for ML work. You'll thank me later!


## 📊 Essential Plots for EDA

### 1. Line Plots (Time Series, Training Curves)

```python
fig, ax = plt.subplots(figsize=(10, 6))

# Single line
ax.plot(x, y, label='Training Loss', color='blue', linewidth=2)

# Multiple lines
ax.plot(x, y1, label='Train', color='blue', alpha=0.7)
ax.plot(x, y2, label='Validation', color='red', linestyle='--', alpha=0.7)

ax.set_xlabel('Epoch')
ax.set_ylabel('Loss')
ax.set_title('Training Progress')
ax.legend()
ax.grid(True, alpha=0.3)
plt.show()
```

> [!NOTE]
>**Line Styles**: `-` solid, `--` dashed, `-.` dash-dot, `:` dotted  
>**Markers**: `o` circle, `s` square, `^` triangle, `*` star, `+` plus

### 2. Scatter Plots (Feature Relationships)

```python
fig, ax = plt.subplots(figsize=(8, 6))

# Basic scatter
ax.scatter(x, y, alpha=0.6, s=50)

# With color by category
colors = ['red' if label == 0 else 'blue' for label in labels]
ax.scatter(x, y, c=colors, alpha=0.6, s=50)

# With size variation (bubble chart)
sizes = values * 100
ax.scatter(x, y, c=labels, s=sizes, alpha=0.5, cmap='viridis')
plt.colorbar(label='Target')

ax.set_xlabel('Feature 1')
ax.set_ylabel('Feature 2')
ax.set_title('Feature Scatter Plot')
plt.show()
```

### 3. Histograms (Distributions)

```python
fig, ax = plt.subplots(figsize=(10, 6))

# Single histogram
ax.hist(data, bins=50, alpha=0.7, color='skyblue', edgecolor='black')

# Multiple overlapping
ax.hist(train_data, bins=30, alpha=0.5, label='Train', color='blue')
ax.hist(test_data, bins=30, alpha=0.5, label='Test', color='red')

ax.set_xlabel('Value')
ax.set_ylabel('Frequency')
ax.set_title('Distribution Comparison')
ax.legend()
plt.show()
```

### 4. Box Plots (Outlier Detection)

```python
fig, ax = plt.subplots(figsize=(10, 6))

# Multiple box plots
data_list = [feature1, feature2, feature3]
ax.boxplot(data_list, labels=['Feature 1', 'Feature 2', 'Feature 3'])

ax.set_ylabel('Value')
ax.set_title('Feature Distributions')
ax.grid(True, alpha=0.3)
plt.show()
```

### 5. Heatmaps (Correlation Matrices)

```python
import seaborn as sns  # Easier for heatmaps!

# Correlation matrix
corr_matrix = df.corr()

fig, ax = plt.subplots(figsize=(12, 10))
sns.heatmap(corr_matrix, annot=True, fmt='.2f', cmap='coolwarm', 
            center=0, square=True, ax=ax)
ax.set_title('Feature Correlation Matrix')
plt.tight_layout()
plt.show()
```

### 6. Bar Plots (Feature Importance, Class Distribution)

```python
fig, ax = plt.subplots(figsize=(10, 6))

# Vertical bars
features = ['Age', 'Income', 'Score', 'Experience']
importance = [0.25, 0.35, 0.20, 0.20]

ax.bar(features, importance, color='steelblue', alpha=0.8)
ax.set_ylabel('Importance')
ax.set_title('Feature Importance')
ax.grid(True, alpha=0.3, axis='y')

# Horizontal bars (better for many features)
ax.barh(features, importance, color='coral', alpha=0.8)
ax.set_xlabel('Importance')

plt.show()
```


## 📈 Training Visualizations

### Training & Validation Curves

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

epochs = range(1, len(train_loss) + 1)

# Loss curves
ax1.plot(epochs, train_loss, 'b-', label='Training Loss', linewidth=2)
ax1.plot(epochs, val_loss, 'r--', label='Validation Loss', linewidth=2)
ax1.set_xlabel('Epoch', fontsize=12)
ax1.set_ylabel('Loss', fontsize=12)
ax1.set_title('Model Loss', fontsize=14, fontweight='bold')
ax1.legend(fontsize=11)
ax1.grid(True, alpha=0.3)

# Accuracy curves
ax2.plot(epochs, train_acc, 'b-', label='Training Accuracy', linewidth=2)
ax2.plot(epochs, val_acc, 'r--', label='Validation Accuracy', linewidth=2)
ax2.set_xlabel('Epoch', fontsize=12)
ax2.set_ylabel('Accuracy', fontsize=12)
ax2.set_title('Model Accuracy', fontsize=14, fontweight='bold')
ax2.legend(fontsize=11)
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Learning Rate Schedule

```python
fig, ax = plt.subplots(figsize=(10, 6))

ax.plot(epochs, learning_rates, 'g-', linewidth=2)
ax.set_xlabel('Epoch')
ax.set_ylabel('Learning Rate')
ax.set_title('Learning Rate Schedule')
ax.set_yscale('log')  # Log scale for learning rates
ax.grid(True, alpha=0.3)
plt.show()
```

### Multiple Metrics Dashboard

```python
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

metrics = {
    'Loss': (train_loss, val_loss),
    'Accuracy': (train_acc, val_acc),
    'Precision': (train_prec, val_prec),
    'Recall': (train_rec, val_rec)
}

for ax, (metric_name, (train, val)) in zip(axes.flat, metrics.items()):
    ax.plot(epochs, train, 'b-', label=f'Train {metric_name}', alpha=0.7)
    ax.plot(epochs, val, 'r--', label=f'Val {metric_name}', alpha=0.7)
    ax.set_xlabel('Epoch')
    ax.set_ylabel(metric_name)
    ax.set_title(f'{metric_name} Over Time')
    ax.legend()
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```


## 🎯 Model Evaluation Plots

### Confusion Matrix

```python
from sklearn.metrics import confusion_matrix
import seaborn as sns

# Calculate confusion matrix
cm = confusion_matrix(y_true, y_pred)

# Plot
fig, ax = plt.subplots(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax,
            cbar_kws={'label': 'Count'})
ax.set_xlabel('Predicted Label', fontsize=12)
ax.set_ylabel('True Label', fontsize=12)
ax.set_title('Confusion Matrix', fontsize=14, fontweight='bold')
plt.show()

# Normalized version
cm_normalized = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]
sns.heatmap(cm_normalized, annot=True, fmt='.2%', cmap='Blues', ax=ax)
```

### ROC Curve

```python
from sklearn.metrics import roc_curve, auc

fpr, tpr, thresholds = roc_curve(y_true, y_scores)
roc_auc = auc(fpr, tpr)

fig, ax = plt.subplots(figsize=(8, 6))

ax.plot(fpr, tpr, color='darkorange', lw=2, 
        label=f'ROC curve (AUC = {roc_auc:.2f})')
ax.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--', label='Random')

ax.set_xlim([0.0, 1.0])
ax.set_ylim([0.0, 1.05])
ax.set_xlabel('False Positive Rate')
ax.set_ylabel('True Positive Rate')
ax.set_title('Receiver Operating Characteristic (ROC) Curve')
ax.legend(loc="lower right")
ax.grid(True, alpha=0.3)
plt.show()
```

### Precision-Recall Curve

```python
from sklearn.metrics import precision_recall_curve, average_precision_score

precision, recall, _ = precision_recall_curve(y_true, y_scores)
avg_precision = average_precision_score(y_true, y_scores)

fig, ax = plt.subplots(figsize=(8, 6))

ax.plot(recall, precision, color='purple', lw=2,
        label=f'AP = {avg_precision:.2f}')
ax.set_xlabel('Recall')
ax.set_ylabel('Precision')
ax.set_title('Precision-Recall Curve')
ax.legend(loc="best")
ax.grid(True, alpha=0.3)
plt.show()
```

### Residual Plot (Regression)

```python
residuals = y_true - y_pred

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

# Scatter plot
ax1.scatter(y_pred, residuals, alpha=0.5)
ax1.axhline(y=0, color='r', linestyle='--', linewidth=2)
ax1.set_xlabel('Predicted Values')
ax1.set_ylabel('Residuals')
ax1.set_title('Residual Plot')
ax1.grid(True, alpha=0.3)

# Histogram
ax2.hist(residuals, bins=50, edgecolor='black', alpha=0.7)
ax2.set_xlabel('Residual Value')
ax2.set_ylabel('Frequency')
ax2.set_title('Residual Distribution')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Predicted vs Actual (Regression)

```python
fig, ax = plt.subplots(figsize=(8, 6))

ax.scatter(y_true, y_pred, alpha=0.5)
ax.plot([y_true.min(), y_true.max()], 
        [y_true.min(), y_true.max()], 
        'r--', lw=2, label='Perfect Prediction')

ax.set_xlabel('True Values')
ax.set_ylabel('Predicted Values')
ax.set_title('Predicted vs Actual')
ax.legend()
ax.grid(True, alpha=0.3)
plt.show()
```


## 🔲 Subplots & Layouts

### Basic Subplots

```python
# 2x2 grid
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Access individual plots
axes[0, 0].plot(x, y1)
axes[0, 1].scatter(x, y2)
axes[1, 0].hist(data)
axes[1, 1].bar(categories, values)

# Flatten for easy iteration
for ax in axes.flat:
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Different Sized Subplots

```python
# Using GridSpec for flexibility
from matplotlib.gridspec import GridSpec

fig = plt.figure(figsize=(12, 8))
gs = GridSpec(3, 3, figure=fig)

ax1 = fig.add_subplot(gs[0, :])    # Top row, all columns
ax2 = fig.add_subplot(gs[1, :-1])  # Middle row, first 2 columns
ax3 = fig.add_subplot(gs[1:, -1])  # Last column, bottom 2 rows
ax4 = fig.add_subplot(gs[-1, 0])   # Bottom left
ax5 = fig.add_subplot(gs[-1, 1])   # Bottom middle

# Plot on each
ax1.plot(x, y)
ax2.scatter(x, y)
# ... etc

plt.tight_layout()
plt.show()
```

### Side-by-Side Comparison

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

ax1.plot(x, y1, 'b-', label='Model 1')
ax1.set_title('Model 1 Performance')
ax1.legend()

ax2.plot(x, y2, 'r-', label='Model 2')
ax2.set_title('Model 2 Performance')
ax2.legend()

plt.tight_layout()
plt.show()
```

### Shared Axes

```python
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 8), sharex=True)

ax1.plot(epochs, train_loss, label='Training')
ax1.plot(epochs, val_loss, label='Validation')
ax1.set_ylabel('Loss')
ax1.legend()

ax2.plot(epochs, train_acc, label='Training')
ax2.plot(epochs, val_acc, label='Validation')
ax2.set_xlabel('Epoch')
ax2.set_ylabel('Accuracy')
ax2.legend()

plt.tight_layout()
plt.show()
```


## 🎨 Styling & Themes

### Built-in Styles

```python
# See all available styles
print(plt.style.available)

# Use a style
plt.style.use('seaborn-v0_8-darkgrid')  # Updated for matplotlib 3.9+
# Other good ones: 'ggplot', 'fivethirtyeight', 'bmh', 'seaborn-v0_8-paper'

# Or use with context
with plt.style.context('seaborn-v0_8-darkgrid'):
    plt.plot(x, y)
    plt.show()
```

### Seaborn Integration (Recommended!)

```python
import seaborn as sns

# Set seaborn style
sns.set_style('whitegrid')  # 'darkgrid', 'white', 'dark', 'ticks'
sns.set_context('notebook')  # 'paper', 'talk', 'poster'

# Custom color palette
sns.set_palette('husl')  # 'deep', 'muted', 'bright', 'pastel', 'dark'
```

### Custom Colors

```python
# Colormaps for continuous data
# Sequential: 'viridis', 'plasma', 'inferno', 'magma', 'cividis'
# Diverging: 'RdBu', 'RdYlGn', 'coolwarm'
# Qualitative: 'tab10', 'Set1', 'Set2', 'Paired'

plt.scatter(x, y, c=values, cmap='viridis')
plt.colorbar(label='Values')

# Custom colors for categories
colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A']
for i, color in enumerate(colors):
    plt.plot(x, y[i], color=color, label=f'Class {i}')
```

### Global Settings

```python
# Font sizes
plt.rcParams['font.size'] = 12
plt.rcParams['axes.labelsize'] = 14
plt.rcParams['axes.titlesize'] = 16
plt.rcParams['xtick.labelsize'] = 11
plt.rcParams['ytick.labelsize'] = 11
plt.rcParams['legend.fontsize'] = 11

# Figure defaults
plt.rcParams['figure.figsize'] = (10, 6)
plt.rcParams['figure.dpi'] = 100
plt.rcParams['savefig.dpi'] = 300  # High DPI for saving

# Line widths
plt.rcParams['lines.linewidth'] = 2
plt.rcParams['axes.linewidth'] = 1.5

# Grid
plt.rcParams['grid.alpha'] = 0.3
plt.rcParams['grid.linestyle'] = '--'

# Legend
plt.rcParams['legend.frameon'] = True
plt.rcParams['legend.shadow'] = True
```

### Quick Beautiful Plot Template

```python
import seaborn as sns

# Setup
sns.set_style('whitegrid')
sns.set_context('notebook', font_scale=1.2)

fig, ax = plt.subplots(figsize=(10, 6))

# Your plot
ax.plot(x, y, linewidth=2.5, label='Data')

# Styling
ax.set_xlabel('X Label', fontsize=14, fontweight='bold')
ax.set_ylabel('Y Label', fontsize=14, fontweight='bold')
ax.set_title('Beautiful Plot', fontsize=16, fontweight='bold', pad=20)
ax.legend(frameon=True, shadow=True, fontsize=12)
ax.grid(True, alpha=0.3)

# Tight layout
plt.tight_layout()
plt.show()
```


## 💾 Saving Figures for Papers & Reports

### Basic Saving

```python
# High quality PNG
plt.savefig('figure.png', dpi=300, bbox_inches='tight')

# Vector format (scalable, best for papers)
plt.savefig('figure.pdf', format='pdf', bbox_inches='tight')
plt.savefig('figure.svg', format='svg', bbox_inches='tight')

# Transparent background
plt.savefig('figure.png', dpi=300, bbox_inches='tight', transparent=True)
```

### Publication-Ready Settings

```python
# Set publication style
import matplotlib as mpl

mpl.rcParams['font.family'] = 'serif'
mpl.rcParams['font.serif'] = ['Times New Roman']
mpl.rcParams['font.size'] = 11
mpl.rcParams['axes.labelsize'] = 12
mpl.rcParams['axes.titlesize'] = 14
mpl.rcParams['legend.fontsize'] = 10
mpl.rcParams['figure.dpi'] = 150
mpl.rcParams['savefig.dpi'] = 600

# Create figure
fig, ax = plt.subplots(figsize=(6, 4))  # Column width in papers ~6 inches

# Your plot
ax.plot(x, y)

# Save in multiple formats
fig.savefig('figure.pdf', bbox_inches='tight')
fig.savefig('figure.png', dpi=600, bbox_inches='tight')
plt.show()
```

### Batch Saving

```python
def save_figure(fig, filename, formats=['png', 'pdf']):
    """Save figure in multiple formats."""
    for fmt in formats:
        fig.savefig(f'{filename}.{fmt}', 
                   dpi=300 if fmt == 'png' else None,
                   bbox_inches='tight',
                   format=fmt)
    print(f'Saved {filename} in {formats}')

# Use it
fig, ax = plt.subplots()
ax.plot(x, y)
save_figure(fig, 'my_plot', ['png', 'pdf', 'svg'])
plt.close(fig)  # Close to free memory
```


## 🚀 Quick Reference Cheat Sheet

### Creating Figures

```python
# Single plot
fig, ax = plt.subplots(figsize=(10, 6))

# Multiple plots
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Custom layout
from matplotlib.gridspec import GridSpec
fig = plt.figure(figsize=(12, 8))
gs = GridSpec(3, 3, figure=fig)
```

### Essential Plot Types

```python
# Line
ax.plot(x, y, label='Line', color='blue', linewidth=2)

# Scatter
ax.scatter(x, y, c=colors, s=sizes, alpha=0.6, cmap='viridis')

# Bar
ax.bar(x, height, color='steelblue', alpha=0.8)
ax.barh(y, width)  # Horizontal

# Histogram
ax.hist(data, bins=50, alpha=0.7, edgecolor='black')

# Box plot
ax.boxplot([data1, data2], labels=['Group 1', 'Group 2'])

# Heatmap (use seaborn)
sns.heatmap(data, annot=True, cmap='coolwarm', ax=ax)
```

### Customization

```python
# Labels and title
ax.set_xlabel('X Label', fontsize=12)
ax.set_ylabel('Y Label', fontsize=12)
ax.set_title('Title', fontsize=14, fontweight='bold')

# Legend
ax.legend(loc='best', fontsize=11, frameon=True)

# Grid
ax.grid(True, alpha=0.3, linestyle='--')

# Limits
ax.set_xlim(0, 10)
ax.set_ylim(0, 100)

# Scale
ax.set_xscale('log')  # or 'linear', 'symlog'
ax.set_yscale('log')

# Ticks
ax.set_xticks([0, 2, 4, 6, 8, 10])
ax.set_xticklabels(['Zero', 'Two', 'Four', 'Six', 'Eight', 'Ten'])
```

### Common Patterns for ML

```python
# Training curves
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))
ax1.plot(epochs, train_loss, label='Train')
ax1.plot(epochs, val_loss, label='Val')
ax2.plot(epochs, train_acc, label='Train')
ax2.plot(epochs, val_acc, label='Val')

# Feature distributions
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
for ax, feature in zip(axes.flat, features):
    ax.hist(data[feature], bins=30)
    ax.set_title(feature)

# Confusion matrix
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')

# Feature importance
plt.barh(feature_names, importance_scores)
plt.xlabel('Importance')

# Learning curves
from sklearn.model_selection import learning_curve
train_sizes, train_scores, val_scores = learning_curve(model, X, y)
plt.plot(train_sizes, np.mean(train_scores, axis=1), label='Train')
plt.plot(train_sizes, np.mean(val_scores, axis=1), label='Val')
```


## 🎓 Pro Tips for ML Practitioners

### 1. Always Use `tight_layout()`

```python
plt.tight_layout()  # Prevents labels from overlapping
```

### 2. Close Figures to Save Memory

```python
plt.close('all')  # Close all figures
plt.close(fig)    # Close specific figure
```

### 3. Use Seaborn for Quick Beauty

```python
import seaborn as sns
sns.set_style('whitegrid')
sns.set_context('notebook')
# Now all matplotlib plots look better!
```

### 4. Interactive Mode for Notebooks

```python
%matplotlib widget  # Interactive plots
# Use mouse to zoom, pan, etc.
```

### 5. Standard Figure Size

```python
plt.rcParams['figure.figsize'] = (10, 6)  # Good default
```

### 6. Color-blind Friendly Palettes

```python
sns.set_palette('colorblind')  # Accessible to everyone
```

### 7. Quick Plot Function

```python
def quick_plot(data, title='', xlabel='', ylabel=''):
    """Quick plot with sensible defaults."""
    fig, ax = plt.subplots(figsize=(10, 6))
    ax.plot(data, linewidth=2)
    ax.set_title(title, fontsize=14, fontweight='bold')
    ax.set_xlabel(xlabel, fontsize=12)
    ax.set_ylabel(ylabel, fontsize=12)
    ax.grid(True, alpha=0.3)
    plt.tight_layout()
    return fig, ax
```


## 🎯 Real-World ML Workflow Example

```python
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from sklearn.metrics import confusion_matrix, roc_curve, auc

# Setup style
sns.set_style('whitegrid')
plt.rcParams['figure.figsize'] = (12, 8)

# Create dashboard
fig = plt.figure(figsize=(16, 12))
gs = fig.add_gridspec(3, 3, hspace=0.3, wspace=0.3)

# Training curves (top row)
ax1 = fig.add_subplot(gs[0, :2])
ax1.plot(history['loss'], 'b-', label='Train Loss', linewidth=2)
ax1.plot(history['val_loss'], 'r--', label='Val Loss', linewidth=2)
ax1.set_xlabel('Epoch')
ax1.set_ylabel('Loss')
ax1.set_title('Training Progress', fontweight='bold')
ax1.legend()
ax1.grid(True, alpha=0.3)

# Accuracy (top right)
ax2 = fig.add_subplot(gs[0, 2])
ax2.plot(history['accuracy'], 'b-', linewidth=2)
ax2.plot(history['val_accuracy'], 'r--', linewidth=2)
ax2.set_xlabel('Epoch')
ax2.set_ylabel('Accuracy')
ax2.set_title('Accuracy', fontweight='bold')
ax2.grid(True, alpha=0.3)

# Confusion matrix (middle left)
ax3 = fig.add_subplot(gs[1, 0])
cm = confusion_matrix(y_true, y_pred)
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax3)
ax3.set_title('Confusion Matrix', fontweight='bold')

# ROC curve (middle center)
ax4 = fig.add_subplot(gs[1, 1])
fpr, tpr, _ = roc_curve(y_true, y_scores)
roc_auc = auc(fpr, tpr)
ax4.plot(fpr, tpr, 'b-', lw=2, label=f'AUC = {roc_auc:.2f}')
ax4.plot([0, 1], [0, 1], 'r--', lw=2)
ax4.set_xlabel('FPR')
ax4.set_ylabel('TPR')
ax4.set_title('ROC Curve', fontweight='bold')
ax4.legend()
ax4.grid(True, alpha=0.3)

# Feature importance (middle right)
ax5 = fig.add_subplot(gs[1, 2])
ax5.barh(feature_names[:10], importance_scores[:10], color='steelblue')
ax5.set_xlabel('Importance')
ax5.set_title('Top 10 Features', fontweight='bold')
ax5.grid(True, alpha=0.3, axis='x')

# Predictions vs Actual (bottom left & center)
ax6 = fig.add_subplot(gs[2, :2])
ax6.scatter(y_true, y_pred, alpha=0.5)
ax6.plot([y_true.min(), y_true.max()], 
         [y_true.min(), y_true.max()], 'r--', lw=2)
ax6.set_xlabel('True Values')
ax6.set_ylabel('Predicted Values')
ax6.set_title('Predictions vs Actual', fontweight='bold')
ax6.grid(True, alpha=0.3)

# Residuals (bottom right)
ax7 = fig.add_subplot(gs[2, 2])
residuals = y_true - y_pred
ax7.hist(residuals, bins=30, edgecolor='black', alpha=0.7)
ax7.set_xlabel('Residual')
ax7.set_ylabel('Frequency')
ax7.set_title('Residual Distribution', fontweight='bold')
ax7.grid(True, alpha=0.3)

plt.savefig('model_evaluation.png', dpi=300, bbox_inches='tight')
plt.show()
```


## 🎉 You're Ready!

That's it! You now know everything you need to visualize your ML/DL projects like a pro. 

**Remember**:
- Start simple, add complexity as needed
- Use Seaborn for instant beauty
- Save figures early and often
- `tight_layout()` is your friend
- Close figures to save memory

Now go create some beautiful visualizations! 📊✨


## 📚 Quick Resources

- [Matplotlib Docs](https://matplotlib.org/stable/index.html)
- [Seaborn Gallery](https://seaborn.pydata.org/examples/index.html)
- [Matplotlib Cheatsheet](https://matplotlib.org/cheatsheets/)
- [Color Names](https://matplotlib.org/stable/gallery/color/named_colors.html)
- [Colormaps](https://matplotlib.org/stable/tutorials/colors/colormaps.html)

---

<div align="center">
  
**Made with ❤️ for the Python Community by @RajeshTechForge**

</div>
