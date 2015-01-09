# ZDMQueson
开发中遇到的一些问题

1）输出时加该的方法类和名称行数 

    NSLog(@"%@__%s,%d",[NSString stringWithUTF8String:object_getClassName(self)],__FUNCTION__, __LINE__);
   
2）怎么把tableview里cell的小对勾的颜色改成别的颜色？

    tableView.tintColor = [UIColor redColor];
   
3）iOS8中UITableVIew分割线短的问题

    在iOS8中会发现分割线默认是没有全部显示的，在iOS7中适用的代码

   if ([self.myCardTableView respondsToSelector:@selector(separatorInset)]) {
        self.myCardTableView.separatorInset = UIEdgeInsetsZero;
    }
    
   已经不管用了。而要在viewDidLoad中加入已下代码

   if ([self.myCardTableView respondsToSelector:@selector(setLayoutMargins:)]) {
        
        [self.myCardTableView setLayoutMargins:UIEdgeInsetsZero];
        
    }
    
   并且加入UITableView的代理方法
￼
   - (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath
   {
      if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {
          [cell setSeparatorInset:UIEdgeInsetsZero];
      }
    
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        [cell setLayoutMargins:UIEdgeInsetsZero];
    }
    
   }

4）判断设备类型

    CGSize iOSDeviceScreenSize = [[UIScreen mainScreen] bounds].size;

    if ([UIDevice currentDevice].userInterfaceIdiom == UIUserInterfaceIdiomPhone)
    {
        if (iOSDeviceScreenSize.width > 568 || // for iOS devices
            iOSDeviceScreenSize.height > 568) // for iOS simulator
        {   // iPhone 6 and iPhone 6+

            // Instantiate a new storyboard object using the storyboard file named Storyboard_iPhone6

            NSLog(@"loaded iPhone6 Storyboard");
        }
        else if (iOSDeviceScreenSize.width == 568 || // for iOS devices
                 iOSDeviceScreenSize.height == 568) // for iOS simulator
        {   // iPhone 5 and iPod Touch 5th generation: 4 inch screen (diagonally measured)

            // Instantiate a new storyboard object using the storyboard file named Storyboard_iPhone5

            NSLog(@"loaded iPhone5 Storyboard");
        }
        else
        {   // iPhone 3GS, 4, and 4S and iPod Touch 3rd and 4th generation: 3.5 inch screen (diagonally measured)

                // Instantiate a new storyboard object using the storyboard file named Storyboard_iPhone4

                NSLog(@"loaded iPhone4 Storyboard");
        }
    }
    else if ([UIDevice currentDevice].userInterfaceIdiom == UIUserInterfaceIdiomPad)
    {   // The iOS device = iPad

        NSLog(@"loaded iPad Storyboard");
    }
    
5）base64
     
       http://stackoverflow.com/questions/392464/how-do-i-do-base64-encoding-on-iphone-sdk
       
6）系统版本号
   
    if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_6_1) {
        // here you go with iOS 7
    }
         
    NSString *versionString = [[UIDevice currentDevice] systemVersion];
        
    http://stackoverflow.com/questions/448162/determine-device-iphone-ipod-touch-with-iphone-sdk
       
7） 获取当前手机号

     extern NSString* CTSettingCopyMyPhoneNumber();
    +(NSString *) phoneNumber {
       NSString *phone = CTSettingCopyMyPhoneNumber();
       return phone;
    }
    或
    NSString *num = [[NSUserDefaults standardUserDefaults] stringForKey:@"SBFormattedPhoneNumber"];
    
8）在block里用到成员变量或self的，请主动使用__weak

     __weak Class *weakSelf = self;
     但很多时候你不能避免在 Block 中使用 self,在 ARC 以前,你可以使用以下 技巧:
     __block DetailViewController *blockSelf = self;
     self.animatedView.block = ^(CGContextRef context, CGRect rect, CFTimeInterval totalTime, CFTimeInterval deltaTime)
     {
     }
     __block 关键字表示 Block 不 retain 这个变量,因此可以使用 blockSelf.artistName 来访问 artistName 属性,而 Block 也不会捕获 self 对 象。
     
     不过 ARC 中不能使用这个方法,因为变量默认是 strong 引用,即使标记为 __block 也仍然是 strong 类型的引用。这时候__block 的唯一功能是允许你修改 已捕获的变量(没有__block 则变量是只读的)。
     ARC 的解决办法是使用 __weak 变量:
     
     __weak DetailViewController *weakSelf = self;
     self.animatedView.block = ^(CGContextRef context, CGRect rect, CFTimeInterval totalTime, CFTimeInterval deltaTime)
     {
         DetailViewController *strongSelf = weakSelf;
         if (strongSelf != nil)
         {
         };
    }
    weakSelf 变量引用了 self,但不会进行 retain。我们让 Block 捕获 weakSelf 而不是 self,因此不存在所有权回环。但是我们在 Block 中不能直接使用 weakSelf,因为这是一个 weak 指针,当 DetailViewController 释放时它会自动 变成 nil。虽然向 nil 发送 message 是合法的,我们在 Block 中仍然检查了对象 是否存在。这里还有一个技巧,我们临时把 weakSelf 转换为 strong 类型的引 用 strongSelf,这样我们在使用 strongSelf 的时候,可以确保 DetailViewController 不会被其它人释放掉!








     
