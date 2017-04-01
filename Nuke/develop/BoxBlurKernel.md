# 一维访问

## 方框模糊一维内核

我们将要认识的下一个内核是一维的方向模糊内核:

```C++
kernel BoxBlur1D : public ImageComputationKernel<eComponentWise>
{
  Image<eRead, eAccessRanged1D, eEdgeClamped> src;
  Image<eWrite, eAccessPoint> dst;

param:
  int radius;  //The radius of our box blur

local:
  int _filterWidth;

  void define() {
    //RIP node will identify radius as the apron
    defineParam(radius, "Radius", 5); 
  }

  void init() {
    //Set the range we need to access from the source 
    src.setRange(-radius, radius);

    //Set the axis for the 1D-range to be horizontal
    src.setAxis(eX);

    _filterWidth = 2 * radius + 1;
  }

  void process() {
    float sum = 0.0f;
    for(int i = -radius; i <= radius; i++)
      sum += src(i);
    dst() = sum / (float)_filterWidth;
  }
};
```

##   一维访问

