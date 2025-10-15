import streamlit as st
import pandas as pd

st.title("建設日報ダッシュボード（ダミー）")

uploaded = st.file_uploader("CSVをアップロード（daily_reports_dummy.csv）", type=['csv'])
if uploaded:
    df = pd.read_csv(uploaded, encoding='utf-8-sig')
    st.success(f"{df.shape[0]} 行 × {df.shape[1]} 列を読み込みました")

    site = st.selectbox("現場を選択", ["(全て)"] + sorted(df['site_name'].unique().tolist()))
    trade = st.multiselect("職種を選択（複数可）", sorted(df['trade'].unique().tolist()))

    view = df.copy()
    if site != "(全て)":
        view = view[view['site_name'] == site]
    if trade:
        view = view[view['trade'].isin(trade)]

    st.subheader("日別・延べ工数の推移")
    daily = view.groupby('date')['man_hours'].sum().reset_index()
    st.line_chart(daily.set_index('date'))

    st.subheader("作業別・延べ工数（上位10）")
    top_tasks = (view.groupby('task')['man_hours'].sum()
                 .sort_values(ascending=False).head(10))
    st.bar_chart(top_tasks)

    st.subheader("安全関連（ヒヤリ・品質指摘）")
    kpi = view.groupby('date')[['near_miss','qa_issues']].sum()
    st.line_chart(kpi)

else:
    st.info("上のアップローダーでCSVを選択してください。")
