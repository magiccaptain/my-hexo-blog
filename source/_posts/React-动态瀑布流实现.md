---
title: React 动态瀑布流实现
date: 2018-09-20 13:44:30
categories:
  - 前端
tags:
  - React
---

# 测试

```jsx harmony
<div>
  <Row>
    {!loading && (
      <GridLayout
        width={this.props.size.width}
        cols={cols}
        items={50}
        rowHeight={1}
        margin={[10, 0]}
        layouts={layouts}
        onWidthChange={this.onWidthChange}
      >
        {issues.map((i, index) => {
          return (
            <IssueCard
              index={index}
              key={i.id}
              issueId={i.id}
              onHeightChange={this.onIssueCardChange}
            />
          );
        })}
      </GridLayout>
    )}
  </Row>

  {loading && (
    <Row type="flex" justify="center" style={{ marginTop: 20 }}>
      <Spinner fadeIn="none" name="line-scale-pulse-out" color="#1DA57A" />
    </Row>
  )}
</div>
```
