Hi - my name is Eugen and I'm **an HTTP addict**.  
  
I work with HTTP on pretty much every project I implement, day in and day out - and it's a tough habit to break.  
So, for me - sending a request and consuming the response needs to be super easy.  
  
I need to be able to control everything - the type of request (not just the standard GET - **I'm guilty of sending a HEAD request from time to time**).  
I need to set custom headers, specify timeouts, ask for custom Content Types, do Authentication, control Caching - you name it.  
  
And so I tried most of the Java Http Clients out there - and while some of them are nice, none of them are as flexible and mature as **the Apache HttpClient 4** - which is what I'm using most of the time now. Here is [a quick cookbook with my own HttpClient recipes](https://t.dripemail2.com/c/eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJkZXRvdXIiLCJpc3MiOiJtb25vbGl0aCIsInN1YiI6ImRldG91cl9saW5rIiwiaWF0IjoxNjgxMjkwMjE5LCJuYmYiOjE2ODEyOTAyMTksImFjY291bnRfaWQiOiI5NTM5NTU0IiwiZGVsaXZlcnlfaWQiOiJ5c3M3dXg3dWpwdDRrdzc2YWtmaSIsInVybCI6Imh0dHBzOi8vZHJpcC5sYS9jL2V5SmhiR2NpT2lKSVV6STFOaUo5LmV5SmhkV1FpT2lKa1pYUnZkWElpTENKcGMzTWlPaUp0YjI1dmJHbDBhQ0lzSW5OMVlpSTZJbVJsZEc5MWNsOXNhVzVySWl3aWFXRjBJam94TmpNNU1UVTVPRGsxTENKdVltWWlPakUyTXpreE5UazRPVFVzSW1GalkyOTFiblJmYVdRaU9pSTVOVE01TlRVMElpd2lkSEpwWjJkbGNsOXBaQ0k2SWpRM056Z3dNelV5SWl3aVpIbHVZVzFwWTE5MWNtd2lPbTUxYkd3c0luVnliQ0k2SW1oMGRIQnpPaTh2ZDNkM0xtSmhaV3hrZFc1bkxtTnZiUzlvZEhSd1kyeHBaVzUwTkNKOS5idlZBcGVsSEtRS3Y3aXByd0NkNVhEQUstMF9tWmlFOFNSZ2RZb2d2QVhZP2U9dGV2ZXNlazMlNDBnbWFpbC5jb20mX19zPXcwYXcxOTBiYmN3d3l5ZTVwdXBsIn0.tD2qzCKVWDQutR7ZS7V-5YKECqwUagibo_Juc2hn_3k) - to use during development.   
  
[>> GET TO KNOW HTTPCLIENT](https://t.dripemail2.com/c/eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJkZXRvdXIiLCJpc3MiOiJtb25vbGl0aCIsInN1YiI6ImRldG91cl9saW5rIiwiaWF0IjoxNjgxMjkwMjE5LCJuYmYiOjE2ODEyOTAyMTksImFjY291bnRfaWQiOiI5NTM5NTU0IiwiZGVsaXZlcnlfaWQiOiJ5c3M3dXg3dWpwdDRrdzc2YWtmaSIsInVybCI6Imh0dHBzOi8vZHJpcC5sYS9jL2V5SmhiR2NpT2lKSVV6STFOaUo5LmV5SmhkV1FpT2lKa1pYUnZkWElpTENKcGMzTWlPaUp0YjI1dmJHbDBhQ0lzSW5OMVlpSTZJbVJsZEc5MWNsOXNhVzVySWl3aWFXRjBJam94TmpNNU1UVTVPVE14TENKdVltWWlPakUyTXpreE5UazVNekVzSW1GalkyOTFiblJmYVdRaU9pSTVOVE01TlRVMElpd2lkSEpwWjJkbGNsOXBaQ0k2SWpjd05EQTNNREEwTkNJc0ltUjVibUZ0YVdOZmRYSnNJanB1ZFd4c0xDSjFjbXdpT2lKb2RIUndjem92TDNkM2R5NWlZV1ZzWkhWdVp5NWpiMjB2YUhSMGNHTnNhV1Z1ZERRaWZRLlFid0FVYzVaUk85UXU1Wm1rS2JuTEZHVmlvRGVzcV9jZUFKODVtSEFWc0k_ZT10ZXZlc2VrMyU0MGdtYWlsLmNvbSZfX3M9dzBhdzE5MGJiY3d3eXllNXB1cGwifQ.WTXKjJpfICXVVM8V_6XWxIAPOCkeoF1X77laduYkyrM)  
  
**Next,** we're going to talk about timing out with the HttpClient - when we need to. And most of the time - we really should.  
  
  
**P.S.** The story for HttpClient as a consumer of your REST API is quite strong.  
It's this client and rest-assured that I'm using the most throughout my API implementations, and there's really not much I would change.  
If you're doing API work with Spring and want to focus on a full client solution for it, definitely have a look at the main course:  
[>> THE "REST WITH SPRING" MASTER CLASS](https://t.dripemail2.com/c/eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJkZXRvdXIiLCJpc3MiOiJtb25vbGl0aCIsInN1YiI6ImRldG91cl9saW5rIiwiaWF0IjoxNjgxMjkwMjE5LCJuYmYiOjE2ODEyOTAyMTksImFjY291bnRfaWQiOiI5NTM5NTU0IiwiZGVsaXZlcnlfaWQiOiJ5c3M3dXg3dWpwdDRrdzc2YWtmaSIsInVybCI6Imh0dHBzOi8vZHJpcC5sYS9jL2V5SmhiR2NpT2lKSVV6STFOaUo5LmV5SmhkV1FpT2lKa1pYUnZkWElpTENKcGMzTWlPaUp0YjI1dmJHbDBhQ0lzSW5OMVlpSTZJbVJsZEc5MWNsOXNhVzVySWl3aWFXRjBJam94TmpNNU1UWXdNVFV4TENKdVltWWlPakUyTXpreE5qQXhOVEVzSW1GalkyOTFiblJmYVdRaU9pSTVOVE01TlRVMElpd2lkSEpwWjJkbGNsOXBaQ0k2SWprNU9UUTBPRFl3TWlJc0ltUjVibUZ0YVdOZmRYSnNJanB1ZFd4c0xDSjFjbXdpT2lKb2RIUndjem92TDNkM2R5NWlZV1ZzWkhWdVp5NWpiMjB2Y21WemRDMTNhWFJvTFhOd2NtbHVaeTFqYjNWeWMyVWlmUS5XYXE3enA2c2x5WHE5M1JFZUJ2MVZHOWd6cE1ZS0d5RXN0am5hZ2FIVFVNP2U9dGV2ZXNlazMlNDBnbWFpbC5jb20mX19zPXcwYXcxOTBiYmN3d3l5ZTVwdXBsIn0.uZO3hQ2FGO3gZxABLvToOfNynFt5ba4p9jinlp_Ej5k)  
  
Cheers,  
_Eugen_

#javaweekly 