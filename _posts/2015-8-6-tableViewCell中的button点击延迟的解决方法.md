---
layout: post
title: tableViewCell中的button点击延迟的解决方法.md
date: 2015-8-6 13:23:23
categories:
- technology
tags:
- iOS

---


```
- (void) touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [super touchesBegan:touches withEvent:event];    
    [NSOperationQueue.mainQueue addOperationWithBlock:^{ self.highlighted = YES; }];
}
```

```
- (void) touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event
{
    [super touchesCancelled:touches withEvent:event]; 
    [self performSelector:@selector(setDefault) withObject:nil afterDelay:0.1];
}
```

```
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event
{
    [super touchesEnded:touches withEvent:event];    
    [self performSelector:@selector(setDefault) withObject:nil afterDelay:0.1];
}
```

```
- (void)setDefault
{
    [NSOperationQueue.mainQueue addOperationWithBlock:^{ self.highlighted = NO; }]
```