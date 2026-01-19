# juniper--new-setup

✅ Step–1: Router এ Access নিন

Juniper router এ সাধারণত ২ভাবে লগইন করা যায়:

1. Console দিয়ে

RJ45 বা USB console cable

Software: PuTTY / SecureCRT

Speed: 9600

2. SSH দিয়ে

IP দিলে SSH হবে

প্রথমে console দিয়া সেট করতে হয়

✅ Step–2: Initial Setup Mode এ ঢুকুন

Console এ গেলে prompt হবে এমনঃ

root>


system mode এ ঢুকতে লিখুন:

configure

✅ Step–3: Hostname সেট করা
set system host-name ROUTER1

✅ Step–4: User + Password সেট

Default এ root এ password নাই
সেট করুন:

set system root-authentication plain-text-password


Password চাইবে

অথবা new user:

set system login user admin class super-user authentication plain-text-password

✅ Step–5: Management IP সেট

ধরি ge-0/0/0 এ IP দেবেন:

set interfaces ge-0/0/0 unit 0 family inet address 192.168.1.2/24


Gateway:

set routing-options static route 0.0.0.0/0 next-hop 192.168.1.1

✅ Step–6: SSH Enable
set system services ssh

✅ Step–7: Commit

সবশেষে অবশ্যই:

commit


এতে configuration apply হবে

🟦 If Internet WAN Configuration Example

ধরি ISP থেকে পাবলিক IP পেয়েছেন:

set interfaces ge-0/0/1 unit 0 family inet address 103.77.218.10/30
set routing-options static route 0.0.0.0/0 next-hop 103.77.218.9

🟦 If LAN NAT করতে চান (SRX হলে)

Juniper SRX = Firewall + Router

set security zones security-zone trust interfaces ge-0/0/0
set security zones security-zone untrust interfaces ge-0/0/1
set security nat source rule-set OUTBOUND from zone trust to zone untrust
set security nat source rule-set OUTBOUND rule NAT1 match source-address 192.168.1.0/24
set security nat source rule-set OUTBOUND rule NAT1 then source-nat interface

🟦 Status চেক
show interfaces terse
show route
show configuration
