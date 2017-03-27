![Alt text](https://raw.githubusercontent.com/harshilkotecha/UIScrollViewWhenKeyboardAppearInSwift3/master/Screenshot/Simulator_Screen_Shot_21-Mar-2017_11_53_37_AM%20(1).jpg "screenshot")
![Alt text](https://raw.githubusercontent.com/harshilkotecha/UIScrollViewWhenKeyboardAppearInSwift3/master/Screenshot/Simulator_Screen_Shot_21-Mar-2017_11_53_49_AM.jpg "screenshot")


Description :

UIScrollViewWhenKeyboardAppear will translate any UIView up when the keyboard is being shown, then return it when the keyboard is hidden.

when the keyboard is appear view scroll up and when keyboard is dismissed returned to its original position.


This code Supported in :

iPhone keyboard
iPad docked keyboard
iPad undocked keyboard
iPad split keyboard
landscape & protrait
3rd party keyboards
Auto Layout
AutoResizingMask (Springs & Struts)


How to add this one : 

when you have multiple textview it is so difficult so best solution ->

step 1 : add UITextFieldDelegate

class ScrollViewController: UIViewController,UITextFieldDelegate {
step 2 :create new IBOutlet but don't connect with any text field

//  get current text box when user Begin editing
    @IBOutlet weak var activeTextField: UITextField?
step 3 : write this two method when user focus on text filed object pass the reference and store in activeTextField

// get current text field
    func textFieldDidBeginEditing(_ textField: UITextField)
    {
        activeTextField=textField;
    }
    func textFieldDidEndEditing(_ textField: UITextField)
    {
        activeTextField=nil;
    }
step 5 : set Notification in viewdidload setNotificationKeyboard

override func viewWillAppear(_ animated: Bool) {
        // call method for keyboard notification
        self.setNotificationKeyboard()
    }

    // Notification when keyboard show
    func setNotificationKeyboard ()  {
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWasShown(notification:)), name: .UIKeyboardWillShow, object: nil)
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillBeHidden(notification:)), name: .UIKeyboardWillHide, object: nil)
    }
    
    step 5 : two method for hide and unhide

func keyboardWasShown(notification: NSNotification)
    {
        var info = notification.userInfo!
        let keyboardSize = (info[UIKeyboardFrameBeginUserInfoKey] as? NSValue)?.cgRectValue.size
        let contentInsets : UIEdgeInsets = UIEdgeInsetsMake(0.0, 0.0, keyboardSize!.height+10, 0.0)
        self.scrollView.contentInset = contentInsets
        self.scrollView.scrollIndicatorInsets = contentInsets
        var aRect : CGRect = self.view.frame
        aRect.size.height -= keyboardSize!.height
        if let activeField = self.activeTextField
        {
            if (!aRect.contains(activeField.frame.origin))
            {
                self.scrollView.scrollRectToVisible(activeField.frame, animated: true)
            }
        }
    }
    // when keyboard hide reduce height of scroll view
    func keyboardWillBeHidden(notification: NSNotification){
        let contentInsets : UIEdgeInsets = UIEdgeInsetsMake(0.0, 0.0,0.0, 0.0)
        self.scrollView.contentInset = contentInsets
        self.scrollView.scrollIndicatorInsets = contentInsets
        self.view.endEditing(true)
    }
