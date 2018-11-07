# Authentication Test

- UsernamePasswordAuthenticationFilterTests
````
@Test
public void testFailedAuthenticationThrowsException() {
	MockHttpServletRequest request = new MockHttpServletRequest("POST", "/");
	request.addParameter(
			UsernamePasswordAuthenticationFilter.SPRING_SECURITY_FORM_USERNAME_KEY,
			"rod");
	UsernamePasswordAuthenticationFilter filter = new UsernamePasswordAuthenticationFilter();
	AuthenticationManager am = mock(AuthenticationManager.class);
	when(am.authenticate(any(Authentication.class))).thenThrow(
			new BadCredentialsException(""));
	filter.setAuthenticationManager(am);

	try {
		filter.attemptAuthentication(request, new MockHttpServletResponse());
		fail("Expected AuthenticationException");
	}
	catch (AuthenticationException e) {
	}
}
````
````
@Test
public void testNormalOperation() throws Exception {
	MockHttpServletRequest request = new MockHttpServletRequest();
	request.setServletPath("/login");
	request.addParameter("username", "testId");

	AuthenticationFilter filter = new AuthenticationFilter();
	filter.setAuthenticationManager(new AuthenticationManager() {
		public Authentication authenticate(Authentication a) {
			return a;
		}
	});

	assertThat(filter.requiresAuthentication(request, new MockHttpServletResponse())).isTrue();

	Authentication result = filter.attemptAuthentication(request,
			new MockHttpServletResponse());
	assertThat(result != null).isTrue();
}
````
````
@Test(expected = AuthenticationException.class)
public void testNullHandledGracefully() throws Exception {
	AuthenticationFilter filter = new AuthenticationFilter();
	filter.setAuthenticationManager(new AuthenticationManager() {
		public Authentication authenticate(Authentication a) {
			throw new BadCredentialsException("Rejected");
		}
	});

	filter.attemptAuthentication(new MockHttpServletRequest(),
			new MockHttpServletResponse());
}
````
- https://github.com/spring-projects/spring-security