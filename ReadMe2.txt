Angular 2 Notes
01) Data-binding: One way data-binding From Component to View Template
                  One way data-binding From View Template to Component
                  Two way data-binding From Component to View Template & From View template to Component
02) Angular Interpolation: data-bind by using {{yourClassPropertyName or yourClassMethod}}     
    Moves component class properties to view template                                  
03) Property Binding: enclose HTML attribute with square brackets e.g., <img src='{{imagePath}}' /> to <img [src]='imagePath' />
    If you're not a fan of square brackets you can use this variant: <img bind-src'imagePath' />
    Moves component class properties to view template. Angular converts interpolation into property binding under the hood. 
    What's the difference? Why use one over the other? 
    Example where you must use interpolation, concatenate strings: <img src='http://www.PHD.com/{{imagePathPart}}' />
    Example where you must use property binding, set element property to non-string value: <button [disabled]='isDisabled'>isDisabled is boolean</button>
04) Security data sanitization: using interpolation or property binding sanitizes malicious content before displaying it    
05) HTML attribute vs DOM property: When a browser loads a web page it creates a Document Object Model of that page. Keep in mind that Angular binding binds
    to DOM element properties NOT HTML attributes. Property values can change whereas attribute values can't.
    <input id='inputId' type='text' value='Phillip' />, inputId.getAttribute('value') will equal 'Phillip', inputId.value will also equal 'Phillip'.
    If you go into textbox and change to 'Mason', inputId.getAttribute('value') equals 'Phillip' while inputId.value equals 'Mason'.
    HTML attributs and DOM properties are different things. Angular binding works with properties and events, and not attributes
06) Attribute Binding: some HTML attributes don't have equalivent DOM properties and in these instances we need to bind to HTML attributes.
    For example colspan does not have DOM property. <th colspan='2'> will be changed to <th attr.colspan='{{yourColspanProperty}}'>
    You can also use <th [attr.colspan]='yourColspanProperty'> if you prefer brackets
07) Class binding: for CSS, you can use <button class="colorClass" [class]='classesToApply'>, this will replace existing class as well (colorClass).
    <button class="colorClass" [class.boldClass]='applyBoldClass'> will add boldClass if applyBoldClass is true, it will not remove colorClass
    <button class="colorClass italicsClass boldClass" [class.boldClass]='applyBoldClass'> removes boldClass based on applyBoldClass boolean
    addClasses() {
        let classes = {
            boldClass: this.applyBoldClass,
            italicsClass: this.applyItalicsClass
        };
        return classes;
    }
    <button class="colorClass" [ngClass]='addClasses()'> is another variant you can use to handle multiple classes
08) Style Binding: sets inline CSS styles. <button [style.font-weight]="isBold ? 'bold' : 'normal'"> or <button [style.fontWeight]="isBold ? 'bold' : 'normal'">
    <button [style.font-size.px]="fontSize">, <button [ngStyle]="addStyles()"> 
09) Event Binding: <button (click)='onClick()'> or <button on-click='onClick()'>
10) Structual directive to show/hide: <tr *ngIf='showDetails'>
11) Two way data binding: <input [value]='name' (input)=name=$event.target.value' /> or <input [(ngModel)]='name' />
    You need to import FormsModule in app.module.ts to use ngModel
12) Structual directive ngFor: <tr *ngFor="let employee of employees"><td>{{employee.annualSalary}}</td></tr>
    <tr *ngIf="!employees || employees.length == 0"><td>No employees to display</td></tr>
13) Be careful using ngFor with large lists. It will perform poorly. A small change to the list may trigger a cascade of DOM manipulations.     


https://www.youtube.com/watch?v=dEIg-PR4Tew&list=PL6n9fhu94yhWqGD8BuKuX-VTKqlNBj-m6&index=17