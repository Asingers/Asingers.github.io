---
layout: post
title: "iOS UITextField 常规处理"
date: 2017-01-05
author: "Asingers"
header-img: http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-29-Wallions22023.jpeg
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
      
---

    - (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string
    
    {
    
        if ([textField.text rangeOfString:@"."].location == NSNotFound)
        {
            isHaveDian = NO;
        }
        if ([string length] > 0)
        {
            unichar single = [string characterAtIndex:0];//当前输入的字符
            if ((single >= '0' && single <= '9') || single == '.')//数据格式正确
            {
                //首字母不能为0和小数点
                if([textField.text length] == 0)
                {
    
                    if(single == '.')
                    {
    
                        [self showMyMessage:@"亲，第一个数字不能为小数点!"];
    
                        [textField.text stringByReplacingCharactersInRange:range withString:@""];
    
                        return NO;
    
                    }
    
                    if (single == '0')
                    {
    
                        [self showMyMessage:@"亲，第一个数字不能为0!"];
    
                        [textField.text stringByReplacingCharactersInRange:range withString:@""];
    
                        return NO;
    
                    }
    
                }
    
                //输入的字符是否是小数点
    
                if (single == '.')
                {
    
                    if(!isHaveDian)//text中还没有小数点
                    {
    
                        isHaveDian = YES;
    
                        return YES;
    
                    }else{
    
                        [self showMyMessage:@"亲，您已经输入过小数点了!"];
    
                        [textField.text stringByReplacingCharactersInRange:range withString:@""];
    
                        return NO;
    
                    }
    
                }else{
    
                    if (isHaveDian) {//存在小数点
    
                        //判断小数点的位数
    
                        NSRange ran = [textField.text rangeOfString:@"."];
    
                        if (range.location - ran.location <= 2) {
    
                            return YES;
    
                        }else{
    
                            [self showMyMessage:@"亲，您最多输入两位小数!"];
    
                            return NO;
    
                        }
    
                    }else{
    
                        return YES;
    
                    }
    
                }
    
            }else{//输入的数据格式不正确
    
                [self showMyMessage:@"亲，您输入的格式不正确!"];
    
                [textField.text stringByReplacingCharactersInRange:range withString:@""];
    
                return NO;
    
            }
    
        }
    
        else
    
        {
    
            return YES;
    
        }
    
    }
    
    -(void)showMyMessage:(NSString*)aInfo {
    
        UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"提示" message:aInfo delegate:self cancelButtonTitle:@"确定" otherButtonTitles:nil, nil];
    
        [alertView show];
    
    }

