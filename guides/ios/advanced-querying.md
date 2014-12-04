# Advanced Querying
LayerKit provides a flexible and expressive interface with which applications can query for messaging content. Querying is performed with an `LYRQuery` object. To demonstrate a simple example, the following queries LayerKit for the latest 20 messages in the given conversation.

```objectivec
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];
query.predicate = [LYRPredicate predicateWithProperty:@"conversation" operator:LYRPredicateOperatorIsEqualTo value:self.conversation];
query.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"index" ascending:YES]];
query.limit = 20;
query.offset = 0;

NSError *error;
NSOrderedSet *messages = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu messages in conversation", messages.count);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

## Examples

The following examples demonstrate multiple common queryies that can be utilized by applications.

### Conversation Queries

#### All Conversations

```objectivec
// Fetches all LYRConversation objects
LYRQuery *query = [LYRQuery queryWithClass:[LYRConversation class]];

NSError *error;
NSOrderedSet *conversations = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu conversations", conversations.count);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

### Conversations With A Specific User

```objectivec
// Fetches all conversations between the authenticated user and the supplied user
NSArray *participants = @[self.client.authenticatedUserID, @"<USER_ID>"];
LYRQuery *query = [LYRQuery queryWithClass:[LYRConversation class]];
query.predicate = [LYRPredicate predicateWithProperty:@"participants" operator:LYRPredicateOperatorIsEqualTo value:participants];

NSError *error;
NSOrderedSet *conversations = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu conversations with participants %@", conversations.count, participants);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

### Message Queries

#### All Messages

```objectivec
// Fetches all LYRMessage objects
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];

NSError *error;
NSOrderedSet *messages = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu messages", messages.count);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

#### Unread Message Count

```objectivec
// Fetches the count of all unread messages for the authenticated user
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];

// Messages must be unread
LYRPredicate *unreadPredicate =[LYRPredicate predicateWithProperty:@"isUnread" operator:LYRPredicateOperatorIsEqualTo value:@(YES)];

// Messages must not be sent by the authenticated user
LYRPredicate *userPredicate = [LYRPredicate predicateWithProperty:@"sentByUserId" operator:LYRPredicateOperatorIsNotEqualTo value:self.client.authenticatedUserID];

query.predicate = [LYRCompoundPredicate compoundPredicateWithType:LYRCompoundPredicateTypeAnd subpredicates:@[unreadPredicate, userPredicate]];
query.resultType = LYRQueryResultTypeCount;
NSUInteger unreadMessageCount = [self.client countForQuery:query error:nil];
```

#### All Messages IN Conversation

```objectivec
// Fetches all messages for a given conversation
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];
query.predicate = [LYRPredicate predicateWithProperty:@"conversation" operator:LYRPredicateOperatorIsEqualTo value:self.conversation];
query.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"index" ascending:YES]];

NSError *error;
NSOrderedSet *messages = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu messages in conversation", messages.count);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

### Messages From Last Week

```objectivec
// Fetches all messages sent in the last week
NSDate *lastWeek = [[NSDate date] dateByAddingTimeInterval:-60*60*24*7]; // One Week Ago
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];
query.predicate = [LYRPredicate predicateWithProperty:@"sentAt" operator:LYRPredicateOperatorIsGreaterThan value:lastWeek];

NSError *error;
NSOrderedSet *messages = [self.client executeQuery:query error:&error];
if (!error) {
    NSLog(@"%tu messages in conversation", messages.count);
} else {
    NSLog(@"Query failed with error %@", error);
}
```

### Compound Predicates

For more sophisticated queries, applications can utilize the `LYRCompoundQuery` object to specify multiple constraints for a single query. Compound predicates consist of an array of `LYRPredicate` objects which represent individual constraints, in addition to a conjunction operator represented by an `LYRCompoundPredicateType`.

The following demonstrates a compound predicate which will constrain the result set to `LYRMessage` objects that conform to the following criteria:

1. The `conversation` property is equal to the supplied `LYRConversation` object.
2. The `sentByUserID` property is equal to the supplied `<USER_ID>` value.

```objectivec
LYRQuery *query = [LYRQuery queryWithClass:[LYRMessage class]];
LYRPredicate *conversationPredicate = [LYRPredicate predicateWithProperty:@"conversation" operator:LYRPredicateOperatorIsEqualTo value:self.conversation];
LYRPredicate *userPredicate = [LYRPredicate predicateWithProperty:@"sentByUserID" operator:LYRPredicateOperatorIsEqualTo value:@"<USER_ID>"];
query.predicate = [LYRCompoundPredicate compoundPredicateWithType:LYRCompoundPredicateTypeAnd subpredicates:@[userPredicate, conversationPredicate]];

NSUInteger countOfMessages = [self.client countForQuery:query error:&error];
if (!error) {
    NSLog(@"%tu messages matching compound predicate", countOfMessages);
} else {
    NSLog(@"Query failed with error %@", error);
}
```
