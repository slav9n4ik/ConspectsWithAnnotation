**Cors denied in client**
You need to configure the server to not require authorization for OPTIONS requests (that is, the server the request is being sent to — not the one serving your frontend code).

That’s because what’s happening is this:

Your code’s telling your browser it wants to send a request with the Authorization header.
Your browser says, OK, requests with the Authorization header require me to do a CORS preflight OPTIONS to make sure the server allows requests with the Authorization header.
Your browser sends the OPTIONS request to the server without the Authorization header, because the whole purpose of the OPTIONS check is to see if it’s OK to include that header.
Your server sees the OPTIONS request but instead of responding to it in a way that indicates it allows the Authorization header in requests, it rejects it with a 401 since it lacks the header.
Your browser expects a 200 or 204 response for the CORS preflight but instead gets that 401 response. So your browser stops right there and never tries the POST request from your code.
Further details:

The Access-Control-Request-Headers and Access-Control-Request-Method request headers in the screenshot in the question indicate the browser’s doing a CORS preflight OPTIONS request.

And the presence of the Authorization and Content-Type: application/json request headers in your request are what trigger your browser do that CORS preflight — by sending an OPTIONS request to the server before trying the POST request in your code. And because that OPTIONS preflight fails, the browser stops right there and never attempts the POST.

So you must figure out what part of the current server-side code on the server the request is being sent to causes it to require authorization for OPTIONS requests, and change that so it instead responds to OPTIONS with a 200 or 204 success response without authorization being required.

For specific help on doing that for a Spring server in particular, see the following answers:

Disable Spring Security for OPTIONS Http Method
Spring CorsFilter does not seem to work still receiving a 401 on preflight requests
Response for preflight has invalid HTTP status code 401 - Spring
AngularJS & Spring Security with ROLE_ANONYMOUS still returns 401

/**
 * Надо для добавления cors параметров на сервере и разрешить для всех
 */
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurerAdapter() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**").allowedMethods("GET", "POST", "PUT", "DELETE","OPTIONS").allowedOrigins("*")
                        .allowedHeaders("*");
            }
        };
    }
}


