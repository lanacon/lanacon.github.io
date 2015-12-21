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
