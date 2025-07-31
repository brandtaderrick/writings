# Integrating MSAL and Apollo GraphQL in Angular Projects For Easy Client Authentication 2024

This is a short write-up about how to attack the app.config.ts file (because I didn’t find anything online that I liked). In fact, there is basically nothing online for Angular that covers the latest versions. We are running the following:

```
"apollo-angular": "^7.0.0",
"apollo/client": "^3.0.0",
"graphql" : "^16",
```

Some of the articles you find online demonstrate *setup* using the “ApolloModule.” However, this is now deprecated. The new way, via their [https://the-guild.dev/graphql/apollo-angular/docs/get-started](website), is with the “provideApollo()” function.

What Was The Problem?
In short, client authentication and it’s setup. With every request we lob to our backend, we want to send the user’s access token, which we acquire through MSAL. Therefore, we must inject the token into the HTTP Header during interception-time, or something similar.

However, when I tried to get things working with “provideApollo(),” I ended up getting two instances of MSAL on my development branch. Everything was blown to smithereens — I couldn’t access localhost:4200 anymore.

Thus, there is something tricky about configuring the interception within the provideApollo statement, even though I tried my best to follow their documentation.

It should be said.. they do have a section on working with authentication. Access it through their left-side navbar by clicking the “Recipes” dropdown, then “Authentication.” The issue is, however, that one prefabbed solution they offer accesses local storage to grab a token. This is an unsafe practice. In a separate example, they utilize an auth instance, but it seems like they want you to instantiate an actual instance of your auth object in the config file. I didn’t want to do that. I wanted to use the pre-existing auth object created by Angular’s dependency injection system.

## What Was The Solution?
“useFactory()” was the key to solving this. Via the [https://angular.dev/guide/di/dependency-injection-providers](documentation), useFactory allows you access to “a dynamic value based on information available in the DI and elsewhere in the app.” This is the only dependency injection approach available that lets you **peak at information in the Angular injector**. Therefore, it becomes incredibly easy A) to get the token from the MSAL instance, then inject it into the HTTP Header, etc; and B) to read and comprehend the code — because it is simple and relatively concise.

## The Code
```
export const appConfig: ApplicationConfig = {
providers: [
    provideHttpClient(withInterceptorsFromDi(), withFetch()),
    // Provide Apollo
    { provide: Apollo, useClass: Apollo },
    { provide: APOLLO_OPTIONS,
      useFactory: () => {
         const httpLink = inject(HttpLink);
         const auth = inject(MsalService);
         const httpsUrl = httpLink.create({ uri: environment.graphqlEndpoint });
         const account = auth.instance.getAllAccounts()[0];
         let accessToken: AuthenticationResult;

        auth.instance
          .acquireTokenSilent({
            scopes: ['user.read'],
            account: account,
          })
          .then((result) => (accessToken = result));
    
         const middleware = new ApolloLink((operation, forward) => {
             operation.setContext({
                headers: new HttpHeaders().set('Authorization', `Bearer ${accessToken.accessToken || null}`),
              });
             return forward(operation);
         });
    
         const apiLink = middleware.concat(httpsUrl);
    
         return {
           link: apiLink,
           cache: new InMemoryCache(),
         };
      },
         deps: [HttpLink, MsalService],
   },
],
```

## Discussion
We declare some variables, inject some dependencies, grab the logged-in MSAL account, and use middleware to glue the user’s access token to the HTTP Header.

After that, every time a graphql function calls out to a server, our URL (created by this code) is automatically utilized; the Apollo library stores an internal reference to the link devised here.

If you are curious, here is the environment.ts file along with a sample GQL query. We are using .NET and the GraphQL Hot Chocolate framework on our backend, so the query might look different from what you expect.

```
export const environment = {
  graphqlEndpoint: 'http://localhost:7097/graphql/',
}
```
```
 getPricing(productCode: string): any {
    return this.apollo.watchQuery({
      query: gql`
      query {
        pricing(where: { productCode: { eq: "${productCode}" } }) {
          productCode
          available {
            updatedWhen
            pricingDetails {
              productDescription
              displayOrder
              productValue
            }
          }
        }
      }
    `,
    }).valueChanges;
  }
```

In later iterations, I pulled the hard coded query out into a different file, and that results in a setup that looks like this:

```
// --path /src/app/graphql/pricing.ts 
import { gql } from 'apollo-angular';

export const pricing = gql`
  query GetPricing($productCode: String!) {
    pricing(where: { productCode: { eq: $productCode} }) {
      productCode
      available {
        updatedWhen
        pricingDetails {
          productDescription
          displayOrder
          productValue
        }
      }
    }
  }
`;
```
```
import { pricing } from '../../graphql/pricing';

getPricing(productCode: string): Observable<any> {
    return this.apollo
      .watchQuery<any>({
        query: pricing,
        variables: {
          productCode: productCode,
        },
      })
      .valueChanges.pipe(map((result) => result.data.pricing));
  }
```

This yields a bit more readable code(in my opinion), and allows me to manage the cleanliness and intent conveyance a little better.

In the HTML template, all I had to do was use the async pipe. Then I could display the data the way I wanted to.

```
@if(additionalInformation.pricing | async; as addInfo){
  <div class="market-pricing">
    <p>High: {{ addInfo.pricingDetails[1].productValue}}</p>
    <p>Average: {{ addInfo.pricingDetails[0].productValue}}</p>
    <p>Low: {{ addInfo.pricingDetails[2].productValue}}</p>
  </div>
 } @else {
   <div class="market-pricing">
     <p>High: loading...</p>
     <p>Average: loading...</p>
     <p>Low: loading...</p>
   </div>
 }
```

I’ll be the first to admit, I am not sure if this is the most elegant solution, but it certainly works!

## Conclusion
We can easily configure MSAL and Apollo for token-based authentication with our servers. Only if we make use of the longhand providers syntax coupled with the useFactory method, that is.

When we implement the useFactory() method for APOLLO_OPTIONS, we gain access to the **Angular Injector**. This access permits us to “peak” at information already present therein. Ultimately, allowing us to grab the user’s access token via the MSAL instance, then append it to the HTTP Headers.

I didn’t cover the MSAL configuration and setup. I figured that is more well known. If there is interest, I can provide that in a follow-up. I couldn’t find anything online related to the above code, so that’s why I published this in the first place.

If you notice any errors , issues, or room for improvement, please comment and I will make an edit/update.

Have a lovely day.
