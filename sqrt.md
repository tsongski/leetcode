# 求一个数的平方根

今天从一个公众号看到一篇[文章](http://mp.weixin.qq.com/s/0PsaOCR2k566SD9bRtgmlg)，说是面试官面算法， 一道leetcode上的算法变形：求一个数的开方，可以有一定的误差区间。然后就动手在纸上画了画，刚好也在电脑边，就整理了思路，写了出来。检验了一下没有问题，顺手贴出来.

- 思路

求一个数的开方，拿20来说吧，4的平方是16，5的平方是25，那么20的开方肯定是4.* ；

那么我怎么推算出来这个4.* 呢，首先，20的开方，肯定是比20小的嘛(一般来说，一个数的开方确实是应该比它本身小,0~1的数字我们待会另作讨论)，那我要怎么选择一个数字，作为第一个目标值来检验呢？20的话，我从哪个数字开始呢？这里，直觉告诉我，取20的一半，也就是10，10的平方要大于20，那我再取10的一半，发现还是大，那我再取2.5，发现小了。这时候该取几了呢？其实这时候，区间已经缩小到2.5到5之间了。我们取他俩中间的数字，也就是3.75，还是小，再取3.75和5的中间数4.375...如此反复，结果会无限趋近于4.4721...，题目设定可以有一定的误差区间，那么我们的结果只要在这个区间内就可以了。

思路大概就是这样子，0到1区间的思路差不多，代码如下:


    public static float sqrt(int v, double t) {

  		if (v == 0) {
  			return 0;
  		}
  		if (v < 0) {
  			return -1;
  		}
  		float result = 0f;
  		float min = 0, max = 0;
  		while (((v - t) > (result * result)) || ((result * result) > (v + t))) {
  			if (min == 0 && max == 0) {
  				min = v / 2;
  				max = v;
  			}

  			if (min * min > v) {
  				max = min;
  				min = min / 2;
  				result = min;
  			} else if (min * min < v) {
  				float oldMin = min;
  				min = max;
  				max = (oldMin + max) / 2;
  				result = max;
  			} else {
  				return min;
  			}
  			if (min > max) {
  				float temp = max;
  				max = min;
  				min = temp;
  			}
  		}
  		return result;
  	}
