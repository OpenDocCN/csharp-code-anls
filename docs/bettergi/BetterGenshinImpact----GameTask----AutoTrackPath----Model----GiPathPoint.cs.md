# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\Model\GiPathPoint.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Common.Map;
using OpenCvSharp;
using System;

namespace BetterGenshinImpact.GameTask.AutoTrackPath.Model;

/// <summary>
/// Represents a path point
/// Coordinates must be in the game coordinate system
/// </summary>
[Serializable]
public class GiPathPoint
{
    // The point's position in the game coordinate system
    public Point2f Pt { get; set; }

    // The original point used for matching
    public Point2f MatchPt { get; set; }

    // The index of the path point
    public int Index { get; set; }

    // The timestamp when the path point was created
    public DateTime Time { get; set; }

    // The type of the path point, default is Normal
    public string Type { get; set; } = GiPathPointType.Normal.ToString();

    // Creates a GiPathPoint instance from a given point and index
    public static GiPathPoint BuildFrom(Point2f point, int index)
    {
        // Convert the point from map coordinates to game coordinates
        var pt = MapCoordinate.Main2048ToGame(point);
        return new GiPathPoint
        {
            Pt = pt,
            MatchPt = point,
            Index = index,
            Time = DateTime.Now
        };
    }

    // Determines if the given GiPathPoint is a key point
    public static bool IsKeyPoint(GiPathPoint giPathPoint)
    {
        // Checks if the path point type is KeyPoint, Fighting, or Collection
        if (giPathPoint.Type == GiPathPointType.KeyPoint.ToString()
            || giPathPoint.Type == GiPathPointType.Fighting.ToString()
            || giPathPoint.Type == GiPathPointType.Collection.ToString())
        {
            return true;
        }
        return false;
    }
}

// Enumeration of different path point types
public enum GiPathPointType
{
    Normal, // Regular point
    KeyPoint, // Important point
    Fighting, // Combat point
    Collection, // Gathering point
}
```