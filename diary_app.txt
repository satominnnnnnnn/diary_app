import streamlit as st
from datetime import datetime
import os


# 日記を保存するディレクトリ
DIARY_DIR = "diary_entries"
os.makedirs(DIARY_DIR, exist_ok=True)


# タイトル
st.title("日記アプリ")


# 日記の入力フォーム
st.header("新しい日記を書く")
date = st.date_input("日付", datetime.now())
title = st.text_input("タイトル")
content = st.text_area("本文")


if st.button("保存"):
    # 日記を保存するファイル名を生成
    file_name = f"{date}-{title}.txt"
    file_path = os.path.join(DIARY_DIR, file_name)


    # 日記をファイルに保存
    with open(file_path, "w", encoding="utf-8") as file:
        file.write(content)
    
    st.success(f"日記 '{title}' が保存されました")


# 日記を表示するセクション
st.header("過去の日記を見る")
diary_files = os.listdir(DIARY_DIR)
selected_diary = st.selectbox("日記を選択", diary_files)


if selected_diary:
    with open(os.path.join(DIARY_DIR, selected_diary), "r", encoding="utf-8") as file:
        diary_content = file.read()
    st.subheader(selected_diary)
    st.write(diary_content)