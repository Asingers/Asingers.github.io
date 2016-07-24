---
layout: post
title: "导航调起第三方"
date: 2016-07-08
author: "Alpaca"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
---

#### 主要还是通过定位 GEO,unGEO, 获得所在位置 坐标经纬度 和 目的地经纬度,作为参数传出就可以了.
代码:  

     if ([[UIApplication sharedApplication]canOpenURL:[NSURL URLWithString:@"baidumap://"]]){
            
            
            NSString *address = [NSString stringWithFormat:@"%@%@",_destinationPTextField.text,_destinationCTextField.text];
            
            NSString *urlString1 = [[NSString stringWithFormat:@"baidumap://map/direction?origin=latlng:%f,%f|name:我的位置&destination=latlng:%f,%f|name:%@&mode=driving",_startLat, _startLong,_stoptLat,_stoptLong,address] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding] ;
            
            
            
            [[UIApplication sharedApplication]openURL:[NSURL URLWithString:urlString1]];
            
        }else
            
            //拉起高德
            
            if ([[UIApplication sharedApplication]canOpenURL:[NSURL URLWithString:@"iosamap://"]]){
                
                
                //backScheme  设置为应用的scheme，可以返回到自身应用
                NSString *address = [NSString stringWithFormat:@"%@%@",_destinationPTextField.text,_destinationCTextField.text];
                
                NSString *urlString2 = [[NSString stringWithFormat:@"baidumap://map/direction?origin=latlng:%f,%f|name:我的位置&destination=latlng:%f,%f|name:%@&mode=driving",_startLat, _startLong,_stoptLat,_stoptLong,address] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding] ;
                
                
                
                [[UIApplication sharedApplication]openURL:[NSURL URLWithString:urlString2]];
                
            }else{
                
                NSString *address = [NSString stringWithFormat:@"%@%@",_destinationPTextField.text,_destinationCTextField.text];

                NSString *webStr = [[NSString stringWithFormat:@"http://api.map.baidu.com/direction?origin=latlng:%f,%f|name:我的位置&destination=latlng:%f,%f|name:%@&mode=driving&region=我的位置&output=html&src=yourCompanyName|Alpaca",_startLat, _startLong,_stoptLat,_stoptLong,address]  stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
                
                [[UIApplication sharedApplication]openURL:[NSURL URLWithString:webStr]];
                 
            }
            
百度坐标转换:  

	- (NSArray*)exchangeLat:(CGFloat)gg_lat Lon:(CGFloat)gg_lon {
    
    	CGFloat bd_lon;
    	CGFloat bd_lat;
	    const double x_pi = 3.14159265358979324 * 3000.f / 180.f;
    	double x = gg_lon, y = gg_lat;
	    double z = sqrt(x * x + y * y) + 0.00002 * sin(y * x_pi);
    	double theta = atan2(y, x) + 0.000003 * cos(x * x_pi);
	    bd_lon = z * cos(theta) + 0.0065;
    	bd_lat = z * sin(theta) + 0.006;
	    NSArray *array = @[@(bd_lat),@(bd_lon)];
    	return array;
	}

Apple 地图:  

	   // 终点
        CLLocationCoordinate2D coords2 = CLLocationCoordinate2DMake(_stoptLat,_stoptLong);
        
        //当前的位置
        MKMapItem *currentLocation = [MKMapItem mapItemForCurrentLocation];
        
        MKMapItem *toLocation = [[MKMapItem alloc] initWithPlacemark:[[MKPlacemark alloc ]initWithCoordinate:coords2 addressDictionary:nil]];
        toLocation.name = [NSString stringWithFormat:@"%@%@",_destinationPTextField.text,_destinationCTextField.text];
        [MKMapItem openMapsWithItems:@[currentLocation, toLocation]
                       launchOptions:@{MKLaunchOptionsDirectionsModeKey: MKLaunchOptionsDirectionsModeDriving,MKLaunchOptionsMapCenterKey: [NSNumber numberWithBool:YES],MKLaunchOptionsMapTypeKey:[NSNumber numberWithInteger:0]}];