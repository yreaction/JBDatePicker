# JBDatePicker

[![CI Status](http://img.shields.io/travis/Joost/JBDatePicker.svg?style=flat)](https://travis-ci.org/Joost/JBDatePicker)
[![Version](https://img.shields.io/cocoapods/v/JBDatePicker.svg?style=flat)](http://cocoapods.org/pods/JBDatePicker)
[![License](https://img.shields.io/cocoapods/l/JBDatePicker.svg?style=flat)](http://cocoapods.org/pods/JBDatePicker)
[![Platform](https://img.shields.io/cocoapods/p/JBDatePicker.svg?style=flat)](http://cocoapods.org/pods/JBDatePicker)


Overview
==========
* [Description](https://github.com/Tuslareb/JBDatePicker#description)
* [Screenshots](https://github.com/Tuslareb/JBDatePicker#screenshots)
* [GIF Demo](https://github.com/Tuslareb/JBDatePicker#gif-demo)
* [Requirements](https://github.com/Tuslareb/JBDatePicker#requirements)
* [Installation](https://github.com/Tuslareb/JBDatePicker#installation)
* [Usage](https://github.com/Tuslareb/JBDatePicker#usage)
* [Example project](https://github.com/Tuslareb/JBDatePicker#example-project)
* [Author](https://github.com/Tuslareb/JBDatePicker#author)
* [License](https://github.com/Tuslareb/JBDatePicker#license)


Description
==========
JBDatePicker is a subclass of UIView, written in Swift 3, that shows a calendarmonth in which the user can select a date. It allows the user to swipe between months, and to preselect a specific date. It's appearance is largely customizable. See [usage](https://github.com/Tuslareb/JBDatePicker#usage) for more information.

Screenshots
==========

GIF Demo
==========
<p align="center">
<img src ="https://cloud.githubusercontent.com/assets/6486085/21116844/8c9fd570-c0b7-11e6-8041-6206b540a26a.gif" />
</p>

Requirements
==========
* ARC
* iOS9
* Swift 3

Installation
==========
<h3> CocoaPods </h3>

```ruby
pod "JBDatePicker"
```
Usage
==========
JBDatePicker can be implemented in your project in two ways:
* Storyboard setup
* Manual setup

**Important**: whatever setup you choose, JBDatePicker needs two things to work correctly:
+ *Your viewController needs to adopt the JBDatePickerDelegate protocol.*
+ *Your viewController has to override the ‘viewDidLayoutSubviews’ method in which you need to call ‘updateLayout()’ on the JBDatePicker object.*

Besides that, you need to integrate **JBDatePicker** with your project through **CocoaPods**. If you don’t know how to do this, please follow this tutorial. Alternatively you can drag the JBDatePicker development classes into your project manually. 

###Storyboard setup

Add a UIView to your storyboard, go to the identity inspector and select JBDatePickerView as the custom class for your new view. Next, open the assistant editor and control drag to your viewController to create an outlet like this:

```swift
@IBOutlet weak var datePicker: JBDatePickerView!
```

Next, make sure you adopt the ‘JBDatePickerViewDelegate’ protocol and set your viewController as the delegate of JBDatePicker, for example in viewDidLoad:

```swift
override func viewDidLoad() {
super.viewDidLoad()

datePicker.delegate = self
}
```

Adopting the JBDatePickerViewDelegate protocol requires the implementation of a method called didSelectDay(dayView:):

```swift
// MARK: - JBDatePickerViewDelegate implementation

func didSelectDay(_ dayView: JBDatePickerDayView) {
print("date selected: \(dayView.date)")
}
```

Finally, implement the viewDidLayoutSubviews method and call the updateLayout() method:

```swift
override func viewDidLayoutSubviews() {
super.viewDidLayoutSubviews()

datePicker.updateLayout()
}
```

Running the app should show JBDatePicker and tapping a date should print a statement in the console. If not, double check that you’ve set the delegate and that you called the updateLayout method. If you want to customize the looks of JBDatePicker, keep reading. 

###Manual setup

It is also possible to setup JBDatePicker without using Interface Builder. This is a code example: 

```swift
class ViewController: UIViewController, JBDatePickerViewDelegate {

var datePicker: JBDatePickerView!

override func viewDidLoad() {
super.viewDidLoad()

let frameForDatePicker = CGRect(x: 0, y: 20, width: view.bounds.width, height: 250)
datePicker = JBDatePickerView(frame: frameForDatePicker)
view.addSubview(datePicker)
datePicker.delegate = self  
}

override func viewDidLayoutSubviews() {
super.viewDidLayoutSubviews()

datePicker.updateLayout()
}

// MARK: - JBDatePickerViewDelegate

func didSelectDay(_ dayView: JBDatePickerDayView) {
print("date selected: \(dayView.date)")
}
```

###Delegate functionality

Besides the only required method, the delegate offers several optional methods to implement:

```swift
/**
Is called when the user swiped (or manually moved) to another month
- parameter monthView: the monthView that is now 'on screen'
*/
func didPresentOtherMonth(_ monthView: JBDatePickerMonthView) {
print(“month selected: \(monthView.monthDescription)”)
}

///Sets the day that determines which month is shown on initial load. Defaults to the current date.
var dateToShow: Date { return a Date object}
```

###Appearance customization

It is possible to customize the appearance of several parts of JBDatePicker by implementing the following optional methods:

```swift
///Sets the first day of the week. Defaults to the local preference.
var firstWeekDay: JBWeekDay { return .wednesday }

///Determines if a month should also show the dates of the previous and next month. Defaults to true.
var shouldShowMonthOutDates: Bool { return false }

///Determines the height ratio of the weekDaysView compared to the total height. Defaults to 0.1 (10%).
var weekDaysViewHeightRatio: CGFloat { return 0.2 }

///color of any date label text that falls within the presented month
var colorForDayLabelInMonth: UIColor { return UIColor of choice }

///color of any date label text that falls out of the presented month and is part of the next or previous (but not presented) month
var colorForDayLabelOutOfMonth: UIColor { return UIColor of choice }

///color of the 'today' date label text
var colorForCurrentDay: UIColor { return //UIColor of choice }

///color of any label text that is selected
var colorForSelelectedDayLabel: UIColor { return UIColor of choice }

///color of the bar which shows the 'mon' to 'sun' labels. Defaults to green. 
var colorForWeekDaysViewBackground: UIColor { return UIColor of choice }

///color of the labels in the WeekdaysView bar that say 'mon' to 'sun'. Defaults to white.
var colorForWeekDaysViewText: UIColor { return UIColor of choice }

///color of the selection circle for dates that aren't today
var colorForSelectionCircleForOtherDate: UIColor { return UIColor of choice }

///color of the selection circle for today
var colorForSelectionCircleForToday: UIColor { return UIColor of choice }

///color of the semi selected selection circle (that shows on a long press)
var colorForSemiSelectedSelectionCircle: UIColor { return UIColor of choice }
```

Example Project
==========
An example project is included with this repo. To run the example project, clone the repo, and run `pod install` from the Example directory first.

Author
==========
Joost van Breukelen

License
==========
JBDatePicker is available under the MIT license. See the LICENSE file for more info.
