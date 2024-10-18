import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def load_data(file_path):
    df = pd.read_csv(file_path, parse_dates=['date'], index_col='date')
    return df

def clean_data(df):
    lower_bound = df['value'].quantile(0.025)
    upper_bound = df['value'].quantile(0.975)
    
    df_filtered = df[(df['value'] >= lower_bound) & (df['value'] <= upper_bound)]
    return df_filtered

def draw_line_plot(df):
    plt.figure(figsize=(10, 5))
    plt.plot(df.index, df['value'], color='blue', linewidth=1)
    plt.title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
    plt.xlabel('Date')
    plt.ylabel('Page Views')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig('line_plot.png')
    plt.show()

def draw_bar_plot(df):
    df['year'] = df.index.year
    df['month'] = df.index.month_name()
    
    avg_page_views = df.groupby(['year', 'month'])['value'].mean().unstack()
    
    plt.figure(figsize=(12, 6))
    avg_page_views.plot(kind='bar', ax=plt.gca())
    plt.title('Average Daily Page Views per Month')
    plt.xlabel('Years')
    plt.ylabel('Average Page Views')
    plt.legend(title='Months')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig('bar_plot.png')
    plt.show()

def draw_box_plot(df):
    df['year'] = df.index.year
    df['month'] = df.index.month
    
    month_order = ['January', 'February', 'March', 'April', 'May', 'June',
                   'July', 'August', 'September', 'October', 'November', 'December']
    
    plt.figure(figsize=(14, 6))
    
    plt.subplot(1, 2, 1)
    sns.boxplot(x='year', y='value', data=df)
    plt.title('Year-wise Box Plot (Trend)')
    plt.xlabel('Year')
    plt.ylabel('Page Views')
    
    plt.subplot(1, 2, 2)
    sns.boxplot(x='month', y='value', data=df, order=month_order)
    plt.title('Month-wise Box Plot (Seasonality)')
    plt.xlabel('Month')
    plt.ylabel('Page Views')
    
    plt.tight_layout()
    plt.savefig('box_plot.png')
    plt.show()

def main():
    df = load_data('fcc-forum-pageviews.csv')
    df_cleaned = clean_data(df)
    
    draw_line_plot(df_cleaned)
    draw_bar_plot(df_cleaned)
    draw_box_plot(df_cleaned)

if __name__ == "__main__":
    main()
