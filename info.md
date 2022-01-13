For Ionic v5 Angular and Capacitor.
<h1> Open PDF saved at assets folder. </h1>

After start the projetc...

Command line 
- npm install cordova-plugin-inappbrowser 
- npm install @awesome-cordova-plugins/in-app-browser
(https://ionicframework.com/docs/native/in-app-browser)

app.module.ts include:

```ruby
import { InAppBrowser } from '@awesome-cordova-plugins/in-app-browser/ngx';
import { HttpClientModule } from '@angular/common/http';
...
@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [HttpClientModule, BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy },
    InAppBrowser],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

home.module.ts include: 

```ruby
import { HttpClientModule } from '@angular/common/http';
...
@NgModule({
...
imports: [HttpClientModule,
...
```

home.page.ts

```ruby
import { InAppBrowser, InAppBrowserOptions } from '@ionic-native/in-app-browser';
import { HttpClient } from '@angular/common/http';
…

options : InAppBrowserOptions = 
    { 
    location : 'yes',//Or 'no' 
    hidden : 'no', //Or  'yes'
    clearcache : 'yes',
    clearsessioncache : 'yes',
    zoom : 'yes',//Android only ,shows browser zoom controls 
    hardwareback : 'yes',
    mediaPlaybackRequiresUserAction : 'no',
    shouldPauseOnSuspend : 'no', //Android only 
    closebuttoncaption : 'Close', //iOS only
    disallowoverscroll : 'no', //iOS only 
    toolbar : 'yes', //iOS only 
    enableViewportScale : 'no', //iOS only 
    allowInlineMediaPlayback : 'no',//iOS only 
    presentationstyle : 'pagesheet',//iOS only 
    fullscreen : 'yes',//Windows only    
    };

url: any;

constructor(
    private iab: InAppBrowser, 
    private http: HttpClient)
{}
…

ngOnInit()
    {
        this.http.get('/assets/file.pdf', { responseType: 'blob'})
          .subscribe(res => {
           const reader = new FileReader();
           reader.onloadend = () => { }
           reader.readAsDataURL(res);
           this.url = URL.createObjectURL(res);
      });
    }
    
openPdf()
    {
        this.iab.create( this.url, "_blank", this.options );
    }
```

home.page.html

```ruby
(...)
  <ion-button expand="full" (click)="openPdf()">Open PDF</ion-button>
(....)
```

References:
- [https://www.techiediaries.com/inappbrowser-ionic-v3/]
- [https://ionicframework.com/docs/native/in-app-browser]
