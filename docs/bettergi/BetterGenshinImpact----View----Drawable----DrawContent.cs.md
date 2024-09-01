# `.\better-genshin-impact\BetterGenshinImpact\View\Drawable\DrawContent.cs`

```cs
﻿using System.Collections.Concurrent;
using System.Collections.Generic;

namespace BetterGenshinImpact.View.Drawable;

public class DrawContent
{
    /// <summary>
    /// 在遮罩窗口上绘制的矩形
    /// </summary>
    // 用于存储在遮罩窗口上绘制的矩形，按键组织，每个键对应一个矩形列表
    public ConcurrentDictionary<string, List<RectDrawable>> RectList { get; set; } = new();

    /// <summary>
    /// 在遮罩窗口上绘制的文本
    /// </summary>
    // 用于存储在遮罩窗口上绘制的文本，按键组织，每个键对应一个文本列表
    public ConcurrentDictionary<string, List<TextDrawable>> TextList { get; set; } = new();

    /// <summary>
    /// 在遮罩窗口上绘制的文本
    /// </summary>
    // 用于存储在遮罩窗口上绘制的线条，按键组织，每个键对应一个线条列表
    public ConcurrentDictionary<string, List<LineDrawable>> LineList { get; set; } = new();

    // 添加一个新的矩形到 RectList 中
    public void PutRect(string key, RectDrawable newRect)
    {
        // 如果 RectList 中已经存在该键的记录
        if (RectList.TryGetValue(key, out var prevRect))
        {
            // 如果之前的矩形列表为空且新矩形与第一个矩形相同，则不进行操作
            if (prevRect.Count == 0 && newRect.Equals(prevRect[0]))
            {
                return;
            }
        }

        // 将新的矩形设置为该键的矩形列表
        RectList[key] = [newRect];
        // 刷新遮罩窗口以反映更改
        MaskWindow.Instance().Refresh();
    }

    // 更新或移除指定键的矩形列表
    public void PutOrRemoveRectList(string key, List<RectDrawable>? list)
    {
        bool changed = false;

        // 如果 RectList 中已经存在该键的记录
        if (RectList.TryGetValue(key, out var prevRect))
        {
            // 如果传入的列表为 null，则移除该键的记录
            if (list == null)
            {
                RectList.TryRemove(key, out _);
                changed = true;
            }
            // 如果之前的矩形列表与传入的列表长度不同，则更新矩形列表
            else if (prevRect.Count != list.Count)
            {
                RectList[key] = list;
                changed = true;
            }
            else
            {
                // 不逐一比较矩形，直接更新列表，因为使用此方法的地方会每帧刷新
                RectList[key] = list;
                changed = true;
            }
        }
        else
        {
            // 如果列表不为空，则将列表添加到 RectList
            if (list is { Count: > 0 })
            {
                RectList[key] = list;
                changed = true;
            }
        }

        // 如果发生了更改，则刷新遮罩窗口
        if (changed)
        {
            MaskWindow.Instance().Refresh();
        }
    }

    // 移除指定键的矩形列表
    public void RemoveRect(string key)
    {
        // 如果 RectList 中存在该键的记录
        if (RectList.TryGetValue(key, out _))
        {
            // 移除该键的记录
            RectList.TryRemove(key, out _);
            // 刷新遮罩窗口
            MaskWindow.Instance().Refresh();
        }
    }

    // 添加一条新的线条到 LineList 中
    public void PutLine(string key, LineDrawable newLine)
    {
        // 如果 LineList 中已经存在该键的记录
        if (LineList.TryGetValue(key, out var prev))
        {
            // 如果之前的线条列表为空且新线条与第一个线条相同，则不进行操作
            if (prev.Count == 0 && newLine.Equals(prev[0]))
            {
                return;
            }
        }

        // 将新的线条设置为该键的线条列表
        LineList[key] = [newLine];
        // 刷新遮罩窗口以反映更改
        MaskWindow.Instance().Refresh();
    }

    // 移除指定键的线条列表
    public void RemoveLine(string key)
    {
        // 如果 LineList 中存在该键的记录
        if (LineList.TryGetValue(key, out _))
        {
            // 移除该键的记录
            LineList.TryRemove(key, out _);
            // 刷新遮罩窗口
            MaskWindow.Instance().Refresh();
        }
    }

    /// <summary>
    /// 清理所有绘制内容
    /// </summary>
    // 清理 RectList、TextList 和 LineList 中的所有内容，并刷新遮罩窗口
    public void ClearAll()
    {
        // 如果所有字典都是空的，则无需清理
        if (RectList.IsEmpty && TextList.IsEmpty && LineList.IsEmpty)
        {
            return;
        }
        // 清除所有绘制内容
        RectList.Clear();
        TextList.Clear();
        LineList.Clear();
        // 刷新遮罩窗口以反映更改
        MaskWindow.Instance().Refresh();
    }
}
```