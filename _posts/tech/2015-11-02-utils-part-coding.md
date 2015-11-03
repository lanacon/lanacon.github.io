---
layout: post
title: ios中经常使用的代码片段
category: 技术
description: ios 开发过程中经常使用到的代码片段.
---

### 通过event 找到tableview 上的某一个cell

    	-(void)didclickBtn_edit:(UIButton *)sender event:(UIEvent *)event

			{

    UITouch *touch = [[event allTouches] anyObject];

    CGPoint currentTouchPosition = [touch locationInView:self.tableview_showdata];

    NSIndexPath *indexPath = [self.tableview_showdata indexPathForRowAtPoint: currentTouchPosition];

    if (indexPath != nil)

    {

        NSLog(@"%ld,%ld",indexPath.row,indexPath.section);

    }
   	}
   	}
   	
* 获取window 上当前最上面的控制器
  		
//获取当前屏幕显示的viewcontroller
    
    	    - (UIViewController *)getCurrentVC
    	{
    UIViewController *result = nil;
     
    UIWindow * window = [[UIApplication sharedApplication] keyWindow];
    if (window.windowLevel != UIWindowLevelNormal)
    {
        NSArray *windows = [[UIApplication sharedApplication] windows];
        for(UIWindow * tmpWin in windows)
        {
            if (tmpWin.windowLevel == UIWindowLevelNormal)
            {
                window = tmpWin;
                break;
            }
        }
    }
     
    UIView *frontView = [[window subviews] objectAtIndex:0];
    id nextResponder = [frontView nextResponder];
     
    if ([nextResponder isKindOfClass:[UIViewController class]])
        result = nextResponder;
    else
        result = window.rootViewController;
     
    return result;
    	}


* 自定义一个view ,自定义样式,这里举例,类似聊天的小泡
![效果图](http://ww3.sinaimg.cn/large/7f5ba233gw1exmru8ng1jj20hq0vkdg6.jpg)

实现代码:
  	  	
	#import "YKPopView.h"

	#define MENUGAP 7
 
	@implementation YKPopView
 
	-(instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];
    if (self) {
        self.backgroundColor = [UIColor clearColor];
    }
    return self;
	}

	// Only override drawRect: if you perform custom drawing.
		// An empty implementation adversely affects performance during animation.
	- (void)drawRect:(CGRect)rect {
    // Drawing code
    CGFloat width = rect.size.width;
    CGFloat height = rect.size.height;
    CGFloat radius = 3;
     
    // 获取CGContext，注意UIKit里用的是一个专门的函数
    CGContextRef context = UIGraphicsGetCurrentContext();
    // 移动到初始点
    CGContextMoveToPoint(context, radius, MENUGAP);
     
    // 绘制第1条线和第1个1/4圆弧
    CGContextAddLineToPoint(context, width * 5/6 - 6,MENUGAP);
    CGContextAddLineToPoint(context, width * 5/6,0);
    CGContextAddLineToPoint(context, width  * 5/6 + 6,MENUGAP);
    CGContextAddLineToPoint(context, width - radius, MENUGAP);
    CGContextAddArc(context, width - radius, radius + MENUGAP, radius, -0.5 * M_PI, 0.0, 0);
     
    // 绘制第2条线和第2个1/4圆弧
    CGContextAddLineToPoint(context, width, height - radius - MENUGAP);
    CGContextAddArc(context, width - radius, height - radius - MENUGAP, radius, 0.0, 0.5 * M_PI, 0);
     
    // 绘制第3条线和第3个1/4圆弧
    CGContextAddLineToPoint(context, radius, height - MENUGAP);
    CGContextAddArc(context, radius, height - radius - MENUGAP, radius, 0.5 * M_PI, M_PI, 0);
     
    // 绘制第4条线和第4个1/4圆弧
    CGContextAddLineToPoint(context, 0, radius + MENUGAP);
    CGContextAddArc(context, radius, radius + MENUGAP, radius, M_PI, 1.5 * M_PI, 0);
     
    // 闭合路径
    CGContextClosePath(context);
     
    [[UIColor redColor] setFill];
    CGContextDrawPath(context, kCGPathFill);
 
	}

* 陆续进行补充....
