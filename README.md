

#import "login.h"

#import "UIConfig.h"

#import <FBSDKCoreKit/FBSDKCoreKit.h>
#import <FBSDKLoginKit/FBSDKLoginKit.h>
#import "registerview.h"

#import "MBProgressHUD.h"







@interface login (){
    UIView *mainView;  //ประกาศ mainView
    
       MBProgressHUD *hud ;
    
    
}


@property (nonatomic, strong) AppDelegate *appDelegate;


-(void)handleFBSessionStateChangeWithNotification:(NSNotification *)notification;

-(void)hideUserInfo:(BOOL)shouldHide;

@end




@implementation login


{
    
    
    
    
    registerview * dialogreg;
    
    
    UIAlertView *loading;
    NSDictionary *dict;
    
    NSString *Email1;
    
    NSString *Email;//ประกาศชื่อ
    
    NSString *idfb ;
    
    
    NSString * name ;
    
    NSMutableArray *_objects;
    
    
    NSString * array ;
    
        
        NSMutableData *receivedData;
        
    

    
}


@synthesize delegate,singleTapGestureRecogniser;




//-(id)init{
//   // self = [super init];
//    if(self){
//        [self.view setBackgroundColor:[UIColor colorWithRed:0 green:0 blue:0 alpha:0.75]];
//        
//    }
//    return self;
//}

 FBSDKLoginManager *login1 ;

- (void)viewDidLoad {
    [super viewDidLoad];
    
     
    [FBSDKLoginButton class];
    

  //  return YES;


    // Do any additional setup after loading the view.
    
    
    UIImageView *bgImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"splashscreen.jpg"]];
    bgImageView.frame = self.view.bounds;
    [self.view addSubview:bgImageView];
    [self.view sendSubviewToBack:bgImageView];
    
    
    hud = [MBProgressHUD showHUDAddedTo:self.view animated:YES];
    hud.mode =  MBProgressHUDModeIndeterminate;
    hud.labelText = @"Loading";
    hud.color = [UIColor grayColor];
    hud.labelColor = [UIColor blackColor]; //
    
    
    
//    self.view.backgroundColor = [[UIColor alloc] initWithPatternImage:[UIImage       imageNamed:@"splashscreen.jpg"]  ];
//    [super viewDidLoad];
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}





-(void) intReg {
    
    dialogreg = [[ registerview alloc ] initWithNibName:@"register" bundle:nil];   // initWithNibName:@"register" bundle:nil] เรียกclass
    [dialogreg setDelegate:self];
    
    
}



-(void) showUIAlertWithMessage:(NSString*)message andTitle:(NSString*)title{
    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title
                                                    message:message
                                                   delegate:self
                                          cancelButtonTitle:@"OK"
                                          otherButtonTitles:nil];
    [alert show];
    
}







-(void)showDialoglogin:(UIView *)targetView{
    mainView =targetView;  //เอามาเท่ากัน 
//  ดลิดเข้าหน้าจอ 
//    singleTapGestureRecogniser = [[UITapGestureRecognizer alloc] initWithTarget:self action:nil];
//    singleTapGestureRecogniser.numberOfTapsRequired = 1;
//    singleTapGestureRecogniser.delegate = self;
//    [self.view  addGestureRecognizer:singleTapGestureRecogniser];
    
    CGFloat statusBarOffset;
    
    if (![[UIApplication sharedApplication] isStatusBarHidden]) {
        // If the status bar is not hidden then we get its height and keep it to the statusBarOffset variable.
        // However, there is a small trick here that needs to be done.
        // In portrait orientation the status bar size is 320.0 x 20.0 pixels.
        // In landscape orientation the status bar size is 20.0 x 480.0 pixels (or 20.0 x 568.0 pixels on iPhone 5).
        // We need to check which is the smallest value (width or height). This is the value that will be kept.
        // At first we get the status bar size.
        CGSize statusBarSize = [[UIApplication sharedApplication] statusBarFrame].size;
        if (statusBarSize.width < statusBarSize.height) {
            // If the width is smaller than the height then this is the value we need.
            statusBarOffset = statusBarSize.width;
        }
        else{
            // Otherwise the height is the desired value that we want to keep.
            statusBarOffset = statusBarSize.height;
        }
    }
    else{
        // Otherwise set it to 0.0.
        statusBarOffset = 0.0;
    }
    
    // Declare the following variables that will take their values
    // depending on the orientation.
    CGFloat width, height, offsetX, offsetY;
    
    if ([[UIApplication sharedApplication] statusBarOrientation] == UIInterfaceOrientationLandscapeLeft ||
        [[UIApplication sharedApplication] statusBarOrientation] == UIInterfaceOrientationLandscapeRight) {
        // If the orientation is landscape then the width
        // gets the targetView's height value and the height gets
        // the targetView's width value.
        width = targetView.frame.size.width;
        height = targetView.frame.size.height;
        
        offsetX = -statusBarOffset;
        offsetY = 0.0;
    }
    else{
        // Otherwise the width is width and the height is height.
        width = targetView.frame.size.width;
        height = targetView.frame.size.height;
        
        offsetX = 0.0;
        offsetY = -statusBarOffset;
    }
    
    // Set the view's frame and add it to the target view.
    [self.view setFrame:CGRectMake(targetView.frame.origin.x, targetView.frame.origin.y, width, height)];
    [self.view setFrame:CGRectOffset(self.view.frame, offsetX, offsetY)];
    [targetView addSubview:self.view];
    
    width = self.dialog_login.frame.size.width;
    height = self.dialog_login.frame.size.height;
    
    offsetX = self.view.frame.size.width/2;
    offsetY = self.view.frame.size.height/2;
    
    
    [self.dialog_login setFrame:CGRectMake(self.view.frame.origin.x, self.view.frame.origin.y, width, height)];
    [self.dialog_login setFrame:CGRectOffset(self.dialog_login.frame, offsetX-170, offsetY-220.5)];
    [self.view addSubview:self.dialog_login];
    
    
    
    // Animate the display of the message view.
    // We change the y point of its origin by setting it to 0 from the -height value point we previously set it.
    [UIView beginAnimations:@"" context:nil];
    [UIView setAnimationDuration:0.5];
    [UIView setAnimationCurve:UIViewAnimationCurveEaseIn];
    [UIView commitAnimations];
}




- (IBAction)bt_loginfb:(id)sender {
    
    
    
    
    FBSDKLoginButton *loginButton = [[FBSDKLoginButton alloc] init];
    [self.view addSubview:loginButton];
    
    
    NSString * str  = @"เก็บเข้าสู้ facebook " ;
    
    
    
    
    Email = @"UserEmail";
    
    //[FBSDKLoginButton class];
    
    login1 = [[FBSDKLoginManager alloc] init];
    [login1 logInWithReadPermissions:@[@"email"] handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
        if (error) {
            // Process error
            
            NSLog(@"  ทำงานไม่ได้ " );
            
        } else if (result.isCancelled) {
            
            NSLog(@"  ทำงานไม่ได้3 " );
            
            // Handle cancellations
        } else {
            // If you ask for multiple permissions at once, you
            // should check if specific permissions missing
            if ([result.grantedPermissions containsObject:@"email"]) {
                
                NSLog(@"  ทำงานได้ g]p " );
                
                
                
                
                
                
                NSData *jsonData = [NSData dataWithContentsOfURL:
                                    
                                    [NSURL URLWithString:@"http://toylection.com/htdocs/selete_list_toy_host.php"]];
                
                id jsonObjects = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:nil];
                
                // values in foreach loop
                
                for (NSDictionary *dataDict in jsonObjects) {
                    
                    
                    
                    NSString *strName = [dataDict objectForKey:@"UserEmail"];
                    
                    
                    
                    dict = [NSDictionary dictionaryWithObjectsAndKeys:
                            
                            strName, Email,
                            nil];
                    
                    NSLog(@"email is %@",strName );
                    
                    
          
                    
                    
                    [[[FBSDKGraphRequest alloc] initWithGraphPath:@"me"
                                                       parameters:@{@"fields": @"picture, email ,id ,name "}]
                     startWithCompletionHandler:^(FBSDKGraphRequestConnection *connection, id result, NSError *error) {
                         if (!error) {
                             NSString *pictureURL = [NSString stringWithFormat:@"%@",[result objectForKey:@"picture"]];
                             
                             NSLog(@"email is %@", [result objectForKey:@"email"]);
                             
                             NSLog(@"id is %@", [result objectForKey:@"id"]);
                             
                             NSLog(@"name is %@", [result objectForKey:@"name"]);
                             
                             
                             
                             
                             Email1 = [result objectForKey:@"email"]; //เก็บ string
                             idfb = [result objectForKey:@"id"];
                             name = [result objectForKey:@"name"];
                             
                             // Show Progress Loading...
                             [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
                             
                             
                             UIAlertView *yes =[[UIAlertView alloc]
                                                initWithTitle:@": ( Yes !"
                                                message:str delegate:self
                                                cancelButtonTitle:@"OK" otherButtonTitles: nil];
                             
                             
                             //เทียบ string
                             if( [  strName isEqualToString: [result objectForKey:@"email"]]){
                                 UIAlertView *completed =[[UIAlertView alloc]
                                                          initWithTitle:@": -) Completed!"
                                                          message:[result objectForKey:@"email"] delegate:self
                                                          cancelButtonTitle:@"OK" otherButtonTitles: nil];
                                 
                                 
                                 
                                 
                                 
                                 
                           //      NSLog(@"เข้าได้แล้ว ",completed);
                                 
                                 [self removeCustomAlertFromViewInstantly ]; //เข้าสู้หน้าจอหลัก
                                 
                                 
                                 
                                 
                                 
                                //  [self removeCustomAlertFromViewInstantly ];
                                 
                                
                                 
                                 
                               //  [self performSegueWithIdentifier: @"loginpass1" sender: self];
                                 
                                 
                                 
                                 
                                 sqlite3_stmt *statement;
                                 const char *dbpath = [_databasePath UTF8String];
                                 
                                 if(sqlite3_open(dbpath, &_DB) == SQLITE_OK){
                                     NSString *insertSQL = [NSString stringWithFormat:@"INSERT INTO user (id_fb,Username,UserEmail,UserPassword,City,Country_toy ) VALUES (\"%@\",\"%@\", \"%@\", \"%@\",\"%@\", \"%@\" )", idfb, name, Email ,@"",@"", @""];
                                     
                                     const char *insert_statement = [insertSQL UTF8String];
                                     sqlite3_prepare_v2(_DB, insert_statement, -1, &statement, NULL);
                                     
                                     if(sqlite3_step(statement) == SQLITE_DONE){
                                         [self showUIAlertWithMessage:@"User added to the database" andTitle:@"Message"];
                                         
                                         
                                         
                                     }
                                     else{
                                         [self showUIAlertWithMessage:@"Failed to add the user" andTitle:@"Error"];
                                     }
                                     sqlite3_finalize(statement);
                                     sqlite3_close(_DB);
                                 }
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 NSMutableString *post =   [NSString stringWithFormat:@"txt_Email=%@& txt_idfb=%@& txt_FirstName=%@ ", Email1 ,idfb ,name  ];
                                 
                                 
                                 
                                 
                                 //  const char *sql_statement = "CREATE TABLE IF NOT EXISTS users (	User_id INTEGER PRIMARY KEY AUTOINCREMENT   Username TEXT,  UserEmail TEXT, UserPassword TEXT,City TEXT,Country_toy TEXT)";
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 // bt_firstname.text = nil ;
                                 
                                 
                                 NSData *postData = [post dataUsingEncoding:NSASCIIStringEncoding allowLossyConversion:YES];
                                 NSString *postLength = [NSString stringWithFormat:@"%d", [postData length]];
                                 
                                 NSURL *url = [NSURL URLWithString:@"http://toylection.com/htdocs/insert_toy.php"];
                                 NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url
                                                                                        cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
                                                                                    timeoutInterval:10.0];
                                 [request setHTTPMethod:@"POST"];
                                 [request setValue:postLength forHTTPHeaderField:@"Content-Length"];
                                 [request setValue:@"application/x-www-form-urlencoded" forHTTPHeaderField:@"Content-Type"];
                                 [request setHTTPBody:postData];
                                 
                                 NSURLConnection *theConnection=[[NSURLConnection alloc] initWithRequest:request delegate:self];
                                 
                                 
                                 
                                 
                                 [completed show];
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                             }
                             
                             
                             
                             
                         }}];
                }
                
            } }
    }];
    
    
    
    
}


    
    

    


- (IBAction)bt_exit:(id)sender {
    
    exit(0);
}

- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldReceiveTouch:(UITouch *)touch
{
    
    if ([touch.view isDescendantOfView:self.dialog_login]) {
        
        return NO;
    }
    else{
        [self removeCustomAlertFromView];
        return YES;
    }
}



-(void)removeCustomAlertFromView{
//    [self.view removeGestureRecognizer:singleTapGestureRecogniser];
    
    // Animate the message view dissapearing.
    [UIView beginAnimations:@"" context:nil];
    [UIView setAnimationDuration:0.5];
    [UIView setAnimationCurve:UIViewAnimationCurveEaseOut];
    
    [UIView commitAnimations];
    
    // Remove the main view from the super view as well after the animation is finished.
    [self.view performSelector:@selector(removeFromSuperview) withObject:nil afterDelay:0];
}



/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/









- (IBAction)bt_reg:(id)sender {
    
//    
//    
//    login *controller = [[login alloc] initWithNibName:@"login" bundle:[NSBundle mainBundle]];
//    [controller.navigationItem setTitle:@"Registration11"];
//    [self.navigationController pushViewController:controller animated:YES];
//   // [controller release];
//    

    
    [self intReg];
        [dialogreg   showDialog_reg:mainView];
    
    [self removeCustomAlertFromViewInstantly];  //ปิด หน้าจอ
    
    }





-(void)removeCustomAlertFromViewInstantly{
    // Just remove the self.view from the superview.
    [self.view removeGestureRecognizer:singleTapGestureRecogniser];
    [self.view removeFromSuperview];
    
}





- (IBAction)bt_login:(id)sender {
    
    
    
    //Name=Poo&Tel=023456789"
    NSMutableString *post = [NSString stringWithFormat:@"email=%@&pass=%@",[self->  _txt_email text],[self->_txt_password text]];
    
    NSData *postData = [post dataUsingEncoding:NSASCIIStringEncoding allowLossyConversion:YES];
    NSString *postLength = [NSString stringWithFormat:@"%d", [postData length]];
    
    NSURL *url = [NSURL URLWithString:@"http://toylection.com/htdocs/login_toy.php"];
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url
                                                           cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
                                                       timeoutInterval:10.0];
    [request setHTTPMethod:@"POST"];
    [request setValue:postLength forHTTPHeaderField:@"Content-Length"];
    [request setValue:@"application/x-www-form-urlencoded" forHTTPHeaderField:@"Content-Type"];
    [request setHTTPBody:postData];
    
    NSURLConnection *theConnection=[[NSURLConnection alloc] initWithRequest:request delegate:self];
    
    
    // Show Progress Loading...
    [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
    
    /*
     loading = [[UIAlertView alloc] initWithTitle:@"" message:@"Please Wait..." delegate:nil cancelButtonTitle:nil otherButtonTitles:nil];
     UIActivityIndicatorView *progress= [[UIActivityIndicatorView alloc] initWithFrame:CGRectMake(125, 50, 30, 30)];
     progress.activityIndicatorViewStyle = UIActivityIndicatorViewStyleWhiteLarge;
     [loading addSubview:progress];
     [progress startAnimating];
     
     [loading show];
     
     
     */
    if (theConnection) {
        self->receivedData = nil;
    } else {
        UIAlertView *connectFailMessage = [[UIAlertView alloc] initWithTitle:@"NSURLConnection " message:@"Failed in viewDidLoad"  delegate: self cancelButtonTitle:@"Ok" otherButtonTitles: nil];
        [connectFailMessage show];
    }
    
}





    
    - (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
    {
        receivedData = [[NSMutableData alloc] init];
    }
    
    - (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data
    {
        sleep(5);
        [receivedData appendData:data];
    }
    
    - (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
    {
        
        
        
        // inform the user
        UIAlertView *didFailWithErrorMessage = [[UIAlertView alloc] initWithTitle: @"NSURLConnection " message: @"didFailWithError"  delegate: self cancelButtonTitle: @"Ok" otherButtonTitles: nil];
        [didFailWithErrorMessage show];
        
        
        //inform the user
        NSLog(@"Connection failed! Error - %@", [error localizedDescription]);
        
    }
    
    - (void)connectionDidFinishLoading:(NSURLConnection *)connection
    {
        // Hide Progress
        
        
         [hud hide:YES];
        
        
        [UIApplication sharedApplication].networkActivityIndicatorVisible = NO;
        [loading dismissWithClickedButtonIndex:0 animated:YES];
        
        // Return Status E.g : { "Status":"1", "Message":"Insert Data Successfully" }
        // 0 = Error
        // 1 = Completed
        
        if(receivedData)
        {
            NSLog(@"%@",receivedData);
            
            NSString *dataString = [[NSString alloc] initWithData:receivedData encoding:NSASCIIStringEncoding];
            NSLog(@"%@",dataString);
            
            id jsonObjects = [NSJSONSerialization JSONObjectWithData:receivedData options:NSJSONReadingMutableContainers error:nil];
            
            // value in key name
            NSString *strStatus = [jsonObjects objectForKey:@"Status"];
            NSString *strMessage = [jsonObjects objectForKey:@"Message"];
            NSLog(@"Status = %@",strStatus);
            NSLog(@"Message = %@",strMessage);
            
            // Completed
            if( [strStatus isEqualToString:@"1"] ){
                UIAlertView *completed =[[UIAlertView alloc]
                                         initWithTitle:@": -) Completed!"
                                         message:strMessage delegate:self
                                         cancelButtonTitle:@"OK" otherButtonTitles: nil];
               
                
                
                // UIViewController *viewController = [[UIViewController alloc] initWithNibName:@"DialogThemeController" bundle:nil];
                
                
             //  [self performSegueWithIdentifier: @"loginpass1" sender: self];
                
                [self removeCustomAlertFromViewInstantly ]; //เข้าสู้หน้าจอหลัก
                
                [completed show];
            }
            else // Error
            {
                UIAlertView *error =[[UIAlertView alloc]
                                     initWithTitle:@": ( Error!"
                                     message:strMessage delegate:self
                                     cancelButtonTitle:@"OK" otherButtonTitles: nil];
                
                
                
             
                [error show];
                
                
                
            }
            
        }
            

}
- (IBAction)bt_FB12:(id)sender {
    
    
    
    FBSDKLoginButton *loginButton = [[FBSDKLoginButton alloc] init];
    [self.view addSubview:loginButton];
    
    
    NSString * str  = @"เก็บเข้าสู้ facebook " ;
    
    
    
    
    Email = @"UserEmail";
    
    //[FBSDKLoginButton class];
    
    login1 = [[FBSDKLoginManager alloc] init];
    [login1 logInWithReadPermissions:@[@"email"] handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
        if (error) {
            // Process error
            
            NSLog(@"  ทำงานไม่ได้ " );
            
        } else if (result.isCancelled) {
            
            NSLog(@"  ทำงานไม่ได้3 " );
            
            // Handle cancellations
        } else {
            // If you ask for multiple permissions at once, you
            // should check if specific permissions missing
            if ([result.grantedPermissions containsObject:@"email"]) {
                
                NSLog(@"  ทำงานได้ g]p " );
                
                
                
                
                
                
                NSData *jsonData = [NSData dataWithContentsOfURL:
                                    
                                    [NSURL URLWithString:@"http://toylection.com/htdocs/selete_list_toy_host.php"]];
                
                id jsonObjects = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:nil];
                
                // values in foreach loop
                
                for (NSDictionary *dataDict in jsonObjects) {
                    
                    
                    
                    NSString *strName = [dataDict objectForKey:@"UserEmail"];
                    
                    
                    
                    dict = [NSDictionary dictionaryWithObjectsAndKeys:
                            
                            strName, Email,
                            nil];
                    
                    NSLog(@"email is %@",strName );
                    
                    
                    
                    
                    
                    [[[FBSDKGraphRequest alloc] initWithGraphPath:@"me"
                                                       parameters:@{@"fields": @"picture, email ,id ,name "}]
                     startWithCompletionHandler:^(FBSDKGraphRequestConnection *connection, id result, NSError *error) {
                         if (!error) {
                             NSString *pictureURL = [NSString stringWithFormat:@"%@",[result objectForKey:@"picture"]];
                             
                             NSLog(@"email is %@", [result objectForKey:@"email"]);
                             
                             NSLog(@"id is %@", [result objectForKey:@"id"]);
                             
                             NSLog(@"name is %@", [result objectForKey:@"name"]);
                             
                             
                             
                             
                             Email1 = [result objectForKey:@"email"]; //เก็บ string
                             idfb = [result objectForKey:@"id"];
                             name = [result objectForKey:@"name"];
                             
                             // Show Progress Loading...
                             [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
                             
                             
                             UIAlertView *yes =[[UIAlertView alloc]
                                                initWithTitle:@": ( Yes !"
                                                message:str delegate:self
                                                cancelButtonTitle:@"OK" otherButtonTitles: nil];
                             
                             
                             //เทียบ string
                             if( [  strName isEqualToString: [result objectForKey:@"email"]]){
                                 UIAlertView *completed =[[UIAlertView alloc]
                                                          initWithTitle:@": -) Completed!"
                                                          message:[result objectForKey:@"email"] delegate:self
                                                          cancelButtonTitle:@"OK" otherButtonTitles: nil];
                                 
                                 
                                 
                                 
                                 
                                 
                                 //      NSLog(@"เข้าได้แล้ว ",completed);
                                 
                                 [self removeCustomAlertFromViewInstantly ]; //เข้าสู้หน้าจอหลัก
                                 
                                 
                                 
                                 
                                 
                                 //  [self removeCustomAlertFromViewInstantly ];
                                 
                                 
                                 
                                 
                                 //  [self performSegueWithIdentifier: @"loginpass1" sender: self];
                                 
                                 
                                 
                                 
                                 sqlite3_stmt *statement;
                                 const char *dbpath = [_databasePath UTF8String];
                                 
                                 if(sqlite3_open(dbpath, &_DB) == SQLITE_OK){
                                     NSString *insertSQL = [NSString stringWithFormat:@"INSERT INTO user (id_fb,Username,UserEmail,UserPassword,City,Country_toy ) VALUES (\"%@\",\"%@\", \"%@\", \"%@\",\"%@\", \"%@\" )", idfb, name, Email ,@"",@"", @""];
                                     
                                     const char *insert_statement = [insertSQL UTF8String];
                                     sqlite3_prepare_v2(_DB, insert_statement, -1, &statement, NULL);
                                     
                                     if(sqlite3_step(statement) == SQLITE_DONE){
                                         [self showUIAlertWithMessage:@"User added to the database" andTitle:@"Message"];
                                         
                                         
                                         
                                     }
                                     else{
                                         [self showUIAlertWithMessage:@"Failed to add the user" andTitle:@"Error"];
                                     }
                                     sqlite3_finalize(statement);
                                     sqlite3_close(_DB);
                                 }
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 NSMutableString *post =   [NSString stringWithFormat:@"txt_Email=%@& txt_idfb=%@& txt_FirstName=%@ ", Email1 ,idfb ,name  ];
                                 
                                 
                                 
                                 
                                 //  const char *sql_statement = "CREATE TABLE IF NOT EXISTS users (	User_id INTEGER PRIMARY KEY AUTOINCREMENT   Username TEXT,  UserEmail TEXT, UserPassword TEXT,City TEXT,Country_toy TEXT)";
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 // bt_firstname.text = nil ;
                                 
                                 
                                 NSData *postData = [post dataUsingEncoding:NSASCIIStringEncoding allowLossyConversion:YES];
                                 NSString *postLength = [NSString stringWithFormat:@"%d", [postData length]];
                                 
                                 NSURL *url = [NSURL URLWithString:@"http://toylection.com/htdocs/insert_toy.php"];
                                 NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url
                                                                                        cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
                                                                                    timeoutInterval:10.0];
                                 [request setHTTPMethod:@"POST"];
                                 [request setValue:postLength forHTTPHeaderField:@"Content-Length"];
                                 [request setValue:@"application/x-www-form-urlencoded" forHTTPHeaderField:@"Content-Type"];
                                 [request setHTTPBody:postData];
                                 
                                 NSURLConnection *theConnection=[[NSURLConnection alloc] initWithRequest:request delegate:self];
                                 
                                 
                                 
                                 
                                 [completed show];
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                                 
                             }
                             
                             
                             
                             
                         }}];
                }
                
            } }
    }];
    
    

    
}
@end

