# Rust in Production: Astropad
In this interview, I talk with Jeremy Knope, Staff Software Engineer at [Astropad](https://astropad.com/), a company that develops products for creative people. Read further to find out where they use Rust, why they’ve chosen it, and what challenges they’ve met while using this language.

Interview with Jeremy Knope

* * *

### 

Could you tell us a little about Astropad, the projects the company works on, and your role there?

Astropad builds products to empower creative people, especially professional creatives, as we started out with a product tailored towards artists.

We now have two products. The first is the Astropad Studio that turns your iPad into a drawing tablet for your computer. The second is Luna Display that turns your iPad into a portable second display for your computer.

My role originally started as helping build the macOS & iPadOS software but grew into helping push Rust to get our products to Windows and doing some Windows development myself.

![](https://serokell.io/files/l2/l2vo41x6.card-1_(1).png)

### 

Can you talk about the tech stack of Astropad and the place of Rust in it?

We call our core stack LIQUID, and it’s the core of our applications that we’ve ported to Rust. It covers networking, video codec, and more. It’s majority Rust on Windows, and eventually will be majority Rust on macOS & iPadOS in the future, as we start replacing more than just networking with the Rust version on those platforms.

We use Rust as our core library, providing a cross-platform engine for all our applications. We’ve also written some internal tools to help with various tasks in Rust.

### 

How and why did you decide to choose Rust?

We decided on Rust after other avenues to port our products to Windows didn’t pan out. We wanted to have the ability to share a codebase across platforms and not maintain separate ones beyond native GUI code. We also required the ability to keep some of our highly tuned C++ & assembly code.

Rust fit this well, and I personally had been learning it and enjoying it quite a bit, so I proposed a staged rewrite in Rust as the way forward. Knowing that rewriting in Rust would likely not lose any performance but possibly gain performance also was a big win.

### 

Any particular Rust libraries that you want to feature?

[`crossbeam_channel`](https://docs.rs/crossbeam-channel/latest/crossbeam_channel/) and [`serde`](https://serde.rs/) are two libraries we use and appreciate. We’ve open-sourced a couple of our own as well, like [`peertalk-rs`](https://docs.rs/peertalk/latest/peertalk/) and [`astro-dnssd`](https://docs.rs/astro-dnssd/latest/astro_dnssd/).

### 

Where is Rust great to use, and where does it fall short in your stack?

I’d say it’s great for what we’re using it for, the core library of our applications. We definitely find getting into the GUI side from Rust a bit rough.

There are some options we’ve used for Windows GUIs, but our main applications remain native GUIs with a Rust FFI to access our library. The FFI is definitely not as ideal as we’d like since being able to stay in pure Rust is much nicer and safer.

### 

What was the biggest challenge while developing your project with Rust?

Bridging parts of our Rust codebase over FFI was one of the biggest challenges. It’s undoubtedly a reminder of what we gain in Rust and tricky to adapt to a C API.

### 

Are you satisfied with the result?

Our Rust C FFI API for our non-Rust frontends isn’t too bad, but we’d prefer to keep the maximum amount of code in Rust. We haven’t been able to have Rust based GUIs at this time beyond some limited tools.

Definitely try to keep your Rust FFI as simple as possible since that’s a safety risk and often an API pain point. There’s a lot of promising tools & libraries out there that may aid in improving & reducing FFI risks. Always more to explore & learn!

### 

What are the future plans of Astropad?

Getting Astropad Studio, our artist-focused solution, released on Windows is our next big thing. This will be great for various artists out there using Windows and wanting to use iPads + Apple pencil with their various creative software.

* * *

I would like to thank Jeremy for the interview and wish Astropad the best of luck in building their amazing products for artists.

If you want to read more articles about Rust, don’t forget to follow us on [Twitter](https://twitter.com/serokell), [Medium](https://serokell.medium.com/), and [DEV](https://dev.to/serokell). 
 [https://serokell.io/blog/rust-in-production-astropad](https://serokell.io/blog/rust-in-production-astropad)
