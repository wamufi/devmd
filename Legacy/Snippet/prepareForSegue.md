---
boostnote:
  createdAt: '2018-12-19T00:55:38.370Z'
  updatedAt: '2020-07-14T07:47:40.940Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: prepareForSegue
  tags:
    - Objective-C
    - iOS
  isStarred: false
  linesHighlighted: []
  key: 8879522a-7908-4cde-abb8-61fe6c943421
  storage: d112f8ec1e85e056a09d
---

prepareForSegue
---
```objc
-(void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    if ([[segue identifier] isEqualToString:@"addCourse"]) {
        
        //ex
        //1. to typical view controller with modal segue
        //AddCourseViewController *acvc = (AddCourseViewController *)[segue destinationViewController];
        
        //2. embed in navigation controller with modal segue
        UINavigationController *nav = [segue destinationViewController];
        AddCourseViewController *acvc = (AddCourseViewController *)[nav topViewController];
        
        acvc.delegate = self;
        
        Course *newCourse = (Course *) [NSEntityDescription insertNewObjectForEntityForName:@"Course" inManagedObjectContext:[self managedObjectContext]];
        
        acvc.currentCourse = newCourse;
    }
    
    if ([[segue identifier] isEqualToString:@"showDetail"]) {

        //3. to typical view controller with push segue
        DisplayEditViewController *dvc = (DisplayEditViewController *) [segue destinationViewController];
        NSIndexPath *indexPath = [self.tableView indexPathForSelectedRow];
        Course *selectedCourse = (Course *) [self.fetchedResultsController objectAtIndexPath:indexPath];
        dvc.currentCourse = selectedCourse;
    }
}
```

Again
When Using Modal Segue
```objc
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([[segue destinationViewController] isEqualToString:@"Modal to MyVC"])
    {
        UINavigationController *nav = [segue destinationViewController];
        MyViewController *vc = [nav topViewController];
        //setup vc
    }
}
```
When Using Push Segue

```objc
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([[segue destinationViewController] isEqualToString:@"Push to MyVC"])
    {
        MyViewController *vc = [segue destinationViewController];
        //setup vc
    }
}
```