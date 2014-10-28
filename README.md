**GCP - General Chat Protocol** 

*WARNING: Specifications are subject to change.*

# About

IRC, in it's current incarnation, is a very powerful communication tool. It's used from small online communities, to powering team collaboration and communication at a large scale like at Facebook. 

But, IRC as a platform has not really grown in a suitable way to meet the needs of it's users. To provide services, user authentication, etc, IRC resorts to *services* to provide this. User authentication is not part of the protocol itself. Services are a hack on the IRCd, at best. Not even the connection between the IRCd and the services, or the functionality of the services, is standardized. Often times, setting up services with an IRCd is a long and painful process. 

User authentication also doesn't provide a way to easily link to other services and online presences, like email, Twitter, Github, or even a Gravatar. It definitely doesn't provide a way for users to find this information, either.

For a constant IRC presence, you also need to use an *IRC bouncer*. This is a proxy between the IRC server and the client that relays and intercepts messages. It manages away states, logging, keeping track of direct messages when your not active, etc. The primary bouncer software, ZNC, can also be rather unreliable and prone to downtime. Most of these things that the bouncer currently handles could be handled in some form on the server.

When setting up a local IRC network, it's almost a requirement to set up an IRCd, services, and a bouncer. That's a mess, right? This is where GCP comes in. GCP is a new protocol that assumes the responsibilities of the IRC protocol. Here are the goals of the GCP project:

- Be an extensible, future-proof, and well implemented protocol.
- Allow for extensive and optional security.
- Merge the roles historically held by the *IRC services* and the *bouncer* into the server.
- Allow for easy but flexible management of a one-piece GCP server.
- Create an easy to use cross-platform GCP client (will probably be web based).
- Allow for and nurture the diversity of use-cases for GCP, ranging from team collaboration to online communities.

# The Protocol

### Message

All data in GCP is sent through messages. There are three types of messages, requests (sent by the client), server messages (sent by the server), and responses (a special kind of server message in which the message is in response to a request). A regular message in GCP looks like this:

`<id>U+0001<type>U+0002<param>U+0003<value>[U+0002<param>U+0003<value>]`

Unicode characters `U+0001` through `U+003` are reserved for the structure of a message. The use of unicode control code characters in the protocol are to ensure content characters cannot interfere with the protocol syntax, thus being designated as splitters of the protocol data.

**id** - \<id>`U+0001`

In the id part of the message, this is a semi-unique id that is given to the request. This id will be returned in the response. Once the id is returned, the id can safely be reused in a new response. If a request has an id that is already in use by the server, a *SYNTAXERROR* type will be the response.

In the case of a non-response server message, the id can be omitted. It will be ignored if included.

**"[]"** - brackets define optional parameters for types, or optional property sets.

**"..."** - amount of the parameters can vary, limit 1 parameter per property set

# Reading this specification

Here is a basic layout of the the parts of the specification:

- **types.md** includes a list of valid types in GCP for both requests and server messages. It also provides examples.
- **errors.md** includes a list of error codes for the GCP protocol.

In all syntax examples, to make the examples clean, the colon (":") will represent a `U+0002` character. The equals sign ("=") will represent a `U+0003` character. ids will be omitted.