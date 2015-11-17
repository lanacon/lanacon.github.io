---
layout: post
title: CoreData 技术
category: 技术
tag: 技术
description: CoreData 技术分享.
---

CoreData 实际上是对 sqlite 的封装.

例子说明:现在有一个选择课程的功能,课程有方向.

比如说: 前端 是一个方向,后台是一个方向.前端的课程有css,js,ios,android.后台的方向有java,c#,php.

表格如下:

| 前台         | 后台          |
| ------------| ------------- |
| css         | java          |
| javascript  | php           |
| iOS         | c#            |

现在我们对前台/后台进行增删改查.

	(1)向前台的课程中添加一个 android  ,想后台的里面添加一个 python.

	(2)查询出前台的所有课程,和后台的所有课程.
	
	(3) 修改 后台课程中的c#  为 C#
	
	(4) 删除前台中的 css 课程.
	
	

分析上面的例子 ,发现 ,前台可以有多个课程,但是一个课程只能属于一个方向(不考虑特殊情况.),那么他们是一对多的关系.


1. 创建空工程.
2. 创建一个单例,CoreData 的增删改查.

3. 单例源码:
    	
    	//
    	#import "SignleCoreData.h"
    	@interface SignleCoreData ()
		@property (readonly, strong, nonatomic) NSManagedObjectModel *managedObjectModel;
			@property (readonly, strong, nonatomic) NSPersistentStoreCoordinator *persistentStoreCoordinator;


		@end

		@implementation SignleCoreData
		#pragma mark - Core Data stack

		@synthesize managedObjectContext = _managedObjectContext;
		@synthesize managedObjectModel = _managedObjectModel;
		@synthesize persistentStoreCoordinator = _persistentStoreCoordinator;
		+ (SignleCoreData *)shareCoreData
		{
		static SignleCoreData * identifier = nil;   
		static dispatch_once_t onceToken;
    			dispatch_once(&onceToken, ^{
        if (identifier == nil) {
            identifier = [[SignleCoreData alloc] init];
        }
          });
             return identifier;
		}

				- (instancetype)init
		{
			self = [super init];
    			if (self) {
        [self managedObjectContext];
        
        NSLog(@"%@",[self applicationDocumentsDirectory]);
        
        }
        return self;
        }
        
    	- (NSURL *)applicationDocumentsDirectory {
    	// The directory the application uses to store the Core Data store file. This code uses a directory named "nationsky.CoreDataSample" in the application's documents directory.
    	return [[[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] lastObject];
    	}

    	- (NSManagedObjectModel *)managedObjectModel {
        // The managed object model for the application. It is a fatal error for the application not to be able to find and load its model.
        if (_managedObjectModel != nil) {
        return _managedObjectModel;
        }
        NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"Model" withExtension:@"momd"];
        _managedObjectModel = [[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL];
        return _managedObjectModel;
    	}

    	- (NSPersistentStoreCoordinator *)persistentStoreCoordinator {
        // The persistent store coordinator for the application. This implementation creates and return a coordinator, having added the store for the application to it.
        if (_persistentStoreCoordinator != nil) {
        return _persistentStoreCoordinator;
        }
    
    
    
    	#pragma mark -------------------ka----------------s
    
        NSDictionary *options = [NSDictionary dictionaryWithObjectsAndKeys:
                             [NSNumber numberWithBool:YES], NSMigratePersistentStoresAutomaticallyOption,
                             [NSNumber numberWithBool:YES], NSInferMappingModelAutomaticallyOption, nil];
    
    
    	#pragma mark -------------------jie----------------s
    
    
        	// Create the coordinator and store

    	    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:[self managedObjectModel]];
    	    
    	    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"CoreDataSample.sqlite"];
        NSError *error = nil;
        NSString *failureReason = @"There was an error creating or loading the application's saved data.";
        if (![_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:options error:&error]) {
        // Report any error we got.
        NSMutableDictionary *dict = [NSMutableDictionary dictionary];
        dict[NSLocalizedDescriptionKey] = @"Failed to initialize the application's saved data";
        dict[NSLocalizedFailureReasonErrorKey] = failureReason;
        dict[NSUnderlyingErrorKey] = error;
        error = [NSError errorWithDomain:@"YOUR_ERROR_DOMAIN" code:9999 userInfo:dict];
        // Replace this with code to handle the error appropriately.
        // abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
        }
    
           return _persistentStoreCoordinator;
    	}


    	- (NSManagedObjectContext *)managedObjectContext {
        // Returns the managed object context for the application (which is already bound to the persistent store coordinator for the application.)
        if (_managedObjectContext != nil) {
        return _managedObjectContext;
        }
    
        NSPersistentStoreCoordinator *coordinator = [self persistentStoreCoordinator];
        if (!coordinator) {
        return nil;
        }
        _managedObjectContext = [[NSManagedObjectContext alloc] initWithConcurrencyType:NSPrivateQueueConcurrencyType];
        [_managedObjectContext setPersistentStoreCoordinator:coordinator];
        return _managedObjectContext;
    	}

    	#pragma mark - Core Data Saving support

    	- (void)saveContext {
        NSManagedObjectContext *managedObjectContext = self.managedObjectContext;
        if (managedObjectContext != nil) {
        NSError *error = nil;
        if ([managedObjectContext hasChanges] && ![managedObjectContext save:&error]) {
            // Replace this implementation with code to handle the error appropriately.
            // abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
            NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
            abort();
        }
        }
    	}
    	}

	
 
    