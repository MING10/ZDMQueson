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

