## Code deprecation

Whenever you decide to deprecate method/class in our codebase, please do:
1) ideally: remove all the usages of this method/class and method/class itself.
2) at least: write proper explanation why you deprecate it and provide an explicit alternative.

> if you make your changes in platform code, consider the impact to all dependent projects.

## Cocoapods

Use only specific pod versions in Podfile

**Preferred:**
```bash
pod 'FBAudienceNetwork', '= 4.0'
```
**Not Preferred:**
```bash
pod 'FBAudienceNetwork', '~> 4.0'
```
## Unit Tests

### Given-When-Then

Structure test cases using **Given-When-Then** pattern.

* The **Given** section describes pre-conditions to the test. 
* The **When** section contains the code we want to test
* The **Then** section contains checks we want to perform

**Given** section may be omitted if there are no specific preconditions.

Comments that separate sections are necessary if test case is too large. In other cases they may be omitted.

Sources:
* https://martinfowler.com/bliki/GivenWhenThen.html
* https://www.objc.io/issues/15-testing/xctest/#given-when-then

**Preferred:**
```swift
func testThat_GivenFilterIsChats_WhenIncomingMessageReceived_ThenUserIsUpdatedWithIncomingMessage() {
    // Given
    let incomingMessage = "incomingMessage"
    self.fakeConnectionsService.filter = .chats
    
    var isUserUpdated = false
    self.fakeConnectionsService.onUpdateUserAtIndex = { (user, index) in
        isUserUpdated = true
        XCTAssertEqual(user.message, incomingMessage)
    }
    
    // When
    self.receive(message: incomingMessage, incoming: true)
    
    // Then
    XCTAssertTrue(isUserUpdated)
}
```

### Tear down

`XCTestCase` instances are kept alive until all tests passed: https://qualitycoding.org/teardown/ Therefore, state should be cleaned in `tearDown()` function.

**Preferred:**
```swift
class UserTests: XCTestCase {

    let sut: User!
    
    override func setUp() {
        super.setUp()
        self.sut = User()
    }
    
    override func tearDown() {
        self.sut = nil
        super.tearDown()
    }
    
    func testThat_UserDoSometing() {
        // use sut  
    }
}
```

**Not Preferred:**
```swift
class UserTests: XCTestCase {

    let sut = User()
    
    func testThat_UserDoSometing() {
        // use sut  
    }
}
```
