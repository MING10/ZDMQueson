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
       
   6） 系统版本号
   
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
