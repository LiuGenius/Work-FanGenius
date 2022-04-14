# PieChart

## PieChart完整示例
其他描述地址  https://www.seotest.cn/jishu/28189.html

```java

        //图表设置
        Description description = new Description();
        description.setEnabled(false);
        mPieChart.setDescription(description);
        //设置半透明圆环的半径, 0为透明
        mPieChart.setTransparentCircleRadius(10f);
        //设置初始旋转角度
        mPieChart.setRotationAngle(-15);

        // 不显示图例
        Legend legend = mPieChart.getLegend();
        legend.setEnabled(false);

        // 和四周相隔一段距离,显示数据
        mPieChart.setExtraOffsets(26, 5, 26, 5);

        // 设置pieChart图表是否可以手动旋转
        mPieChart.setRotationEnabled(true);
        // 设置piecahrt图表点击Item高亮是否可用
        mPieChart.setHighlightPerTapEnabled(true);
        // 设置pieChart图表展示动画效果，动画运行1.4秒结束
        mPieChart.animateY(1400, Easing.EaseInOutQuad);
        //设置pieChart是否只显示饼图上百分比不显示文字
        mPieChart.setDrawEntryLabels(true);
        //是否绘制PieChart内部中心文本
        mPieChart.setDrawCenterText(false);
		
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject object = jsonArray.optJSONObject(i);
            PieEntry pieEntry = new PieEntry(100 / 6, object.optString("name
            entries.add(pieEntry);
        }
        PieDataSet set = new PieDataSet(entries, "Election Results");
		set.setColors(colors);
		//数据连接线距图形片内部边界的距离，为百分数
		set.setValueLinePart1OffsetPercentage(80f);
		//设置连接线的颜色
		set.setValueLineColor(Color.LTGRAY);
		// 连接线在饼状图外面
		set.setYValuePosition(PieDataSet.ValuePosition.OUTSIDE_SLICE);
		// 设置饼块之间的间隔
		set.setSliceSpace(3f);
		set.setHighlightEnabled(true);
		// 绘制内容value，设置字体颜色大小
		set.setDrawValues(true);
		set.setValueFormatter(new PercentFormatter());
		set.setValueTextSize(12f);
		set.setValueTextColor(Color.GRAY);
		PieData data = new PieData(set);
		mPieChart.setData(data);
		mPieChart.invalidate(); // refresh		

```

## 点击事件代码块

```java

mPieChart.setOnChartValueSelectedListener(new OnChartValueSelectedListener() {
	@Override
	public void onValueSelected(Entry e, Highlight h) {
	// e.getX()方法得到x数据
		PieEntry pieEntry = (PieEntry) e;
		Log.d(TAG, "-->value" + pieEntry.getValue() + "->x" + pieEntry.getX() + "->y" + pieEntry.getY());
	}

	@Override
	public void onNothingSelected() {
	}
});

```
