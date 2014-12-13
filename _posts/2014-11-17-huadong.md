 ---

layout: post
title: UI中的图片滑动效果
modified:
categories: blog
excerpt:
tags: iOS
image:
feature:
date: 2014-11-27 20:00:00

 ---





#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    
    UIScrollView * scroll = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, 320, 568)];
    
    for(int i=0;i<=11;i++)
    {
        UIImageView * imgView = [[UIImageView alloc] initWithFrame:CGRectMake(320*i, 0, 320, 210)];
        
        imgView.image = [UIImage imageNamed:[NSString stringWithFormat:@"%d.jpg",i+1]];
        [scroll addSubview:imgView];
        
    }
    [scroll setContentSize:CGSizeMake(320*12, 210)];//总共要翻页数的长度
    scroll.directionalLockEnabled = YES;            //只能沿着一个方向滑动
    scroll.bounces = YES;                            //回弹效果
    scroll.alwaysBounceVertical = NO;               //当图片的大小小于Scroll的容量，保持回弹效果
    scroll.pagingEnabled = YES;                     //滑动时，停留整页
    scroll.showsHorizontalScrollIndicator = NO;     //是否显示水平的滚动条
    scroll.showsVerticalScrollIndicator = YES;      //是否显示垂直的滚动条
    scroll.decelerationRate = 1;                    //滑动速度
    scroll.delegate = self;                         //绑定委托
    [self.view addSubview:scroll];                  //所建立的图片效果贴在view上
    
    UIButton * btn = [UIButton buttonWithType:UIButtonTypeCustom];
    [btn setTitle:@"立即体验" forState:UIControlStateNormal];
    [btn setFrame:CGRectMake(3645, 80, 80, 30)];
    btn.backgroundColor = [UIColor redColor];
    btn.layer.cornerRadius = 5;
    [scroll addSubview:btn];
    
    UIPageControl * page = [[UIPageControl alloc] initWithFrame:CGRectMake(90, 160, 140, 5)];
    currPageControl = page;
    page.numberOfPages = 12;
    page.currentPage = 0;
    page.pageIndicatorTintColor = [UIColor whiteColor];
    page.currentPageIndicatorTintColor = [UIColor redColor];
    [self.view addSubview:page];
    
    
//    UIActivityIndicatorView * aiv = [[UIActivityIndicatorView alloc] initWithFrame:CGRectMake(100, 40, 20, 20)];
//    aiv.activityIndicatorViewStyle = UIActivityIndicatorViewStyleWhiteLarge;
//    [self.view addSubview:aiv];
//    [aiv startAnimating];
}
- (void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    NSLog(@"%f",scrollView.contentOffset.x);
}
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    NSLog(@"scrollViewWillBeginDragging");
}
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    NSLog(@"scrollViewDidEndDragging");
}
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
{
    CGFloat currX = scrollView.contentOffset.x;
    int page = currX / 320;
    currPageControl.currentPage = page;
    NSLog(@"%d",page);
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end