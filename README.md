import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go

# تحميل البيانات
df = pd.read_csv("oil_extraction_data.csv")

# التحقق من البيانات
print("\nملخص البيانات:")
print(df.info())
print("\nالإحصائيات الوصفية:")
print(df.describe())

# تنظيف البيانات وإزالة القيم المفقودة أو غير الصالحة
df.dropna(inplace=True)
df = df[df["Oil_Production"] > 0]  # إزالة القيم السالبة إن وجدت

# تحليل البيانات
plt.figure(figsize=(8, 5))
sns.histplot(df["Oil_Production"], bins=20, kde=True, color='blue')
plt.title("توزيع إنتاج النفط")
plt.xlabel("إنتاج النفط")
plt.ylabel("التكرار")
plt.show()

# تحليل العلاقة بين المتغيرات باستخدام scatter plot
fig = px.scatter(df, x="Pressure", y="Oil_Production", 
                 title="العلاقة بين الضغط وإنتاج النفط",
                 labels={"Pressure": "الضغط", "Oil_Production": "إنتاج النفط"},
                 color=df["Temperature"],
                 size=df["Active_Wells"],
                 hover_data=["Temperature"])
fig.show()

# حفظ التحليل في ملف Excel
df.to_excel("Oil_Extraction_Analysis.xlsx", index=False)
print("تم حفظ التحليل في ملف Excel بنجاح!")
