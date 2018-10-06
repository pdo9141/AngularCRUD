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
14) For large lists, use ngFor with trackBy, this tells Angular to be smarter about modifying DOM.
    <tr *ngFor="let employee of employess; trackBy:trackByEmpCode; let i=index; let isFirst=first; let isLast=last;let isEven=even; let isOdd=odd">
    trackByEmpCode(index: number, employee: any) :string { return employee.code; }
15) Angular Pipes: transform value before display, you can create custom pipes as well. {{employee.code | uppercase}}
    {{employee.dateOfBirth | date:'fullDate' | uppercase}}, {{employee.annualSalary | currency:'USD':true:'1.3-3'}}
16) To pass values from container component to nested component: import Input from angular/core and decorate fields in nested component with @Input()
    <employee-count *ngIf="employees" [all]="getTotalEmployeesCount()" [male]="getTotalMaleEmployeesCount()" (countRadioButtonSelectionChanged)="onEmployeeCountRadoButtonChange($event)"></employee-count>
17) To update container component from nested component from user action: raise event from nested component using Output and EventEmitter from angular core.
    <input type="radio" name="options" value="All" [(ngModel)]="selectedRadioButtonValue" (change)="onRadioButtonSelectionChange()" />
    selectedRadioButtonValue: string = 'All';

    @Output()
    countRadioButtonSelectionChanged: EventEmitter<string> = new EventEmitter<string>();

    onRadioButtonSelectionChange() { this.countRadioButtonSelectionChanged.emit(this.selectedRadioButtonValue); }

    <ng-container *ngFor="let employee of employess;">
        <tr *ngIf="selectedEmployeeCountRadioButton === 'All' || selectedEmployeeCountRadioButton === employee.gender">
            <td></td>
        </tr>
    </ng-container>
18) When do you use Interfaces in Angular: use interfaces to replace the any type to get strong type intellisense. 
    export class Employee implements IEmployee {
        constructor(public code: string, public name: string, public gender: string) {}
    }
19) Angular component lifecycle hooks: ngOnChanges, ngOnInit, ngOnDestroy are most common.
20) When do you use a service in Angular? It's generally used when you need to reuse data or logic across multiple components.
    Import Injectable from angular/core and decorate your service with @Injectable()
    Make service calls in ngOnInit no constructor, make sure you use *ngIf for all components that expect service data to already be returned.
    ngOnInit() { 
        this.employees = this._employeeService.GetEmployees().subscribe(
            (employeeData) => this.employees = employeeData
        ); 
    }
21) Include HttpModule form angular/core to issue web service calls over HTTP. Import Http from angular/http, inject into constructor. 
    import { Injectable } from '@angular/core';
    import { IEmployee } from './employee';    
    import { Http, Response } from '@angular/http';
    import { Observable } from 'rxjs/Observable';
    import 'rxjs/add/operator/map';

    @Injectable()
    export class EmployeeService {
        constuctor(private _http: Http) {}
        getEmployees(): Observable<IEmployee[]> {
            return this._http.get("https://localhost:3000/api/employees")
                .map((response: Response) => <IEmployee[]>response.json())
        }
    }
22) Update your Web.API project to allow CORS:
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="Access-Control-Allow-Origin" value="*" />
                <add name="Access-Control-Allow-Headers" value="Content-Type" />
                <add name="Access-Control-Allow-Methods" value="GET, POST, PUT, DELETE, OPTIONS" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
23) After you rebuild VS solution you'll encounter JIT loading issues, add logic to show loading:
    <tr *ngIf="!employees">
        <td colspan="5">
            Loading data. Please wait...
        </td>
    </tr>    

Continue at video 28
https://www.youtube.com/watch?v=gJKuil24Jag&list=PL6n9fhu94yhWqGD8BuKuX-VTKqlNBj-m6&index=28
