---
title:  "Network ICMP and Sniffing"
layout: single
date:   2021-12-14 19:06:54
author_profile: true
read_time: true
show_date: true
related: true
header:
  teaser: "https://www.layerstack.com/img/docs/resources/pingdiagram2.jpg"
  image: "https://www.layerstack.com/img/docs/resources/pingdiagram2.jpg"
  og_image: "https://www.layerstack.com/img/docs/resources/pingdiagram2.jpg"
categories:
  - Network
tags:
  - Network
  - ICMP
comments: true
share: true
related: true
toc: true
toc_sticky: true
usemathjax: true
---
Requirements:
1) myping sends `ICMP ECHO REQUEST` and receives `ICMP-ECHO-REPLY` (one
time is enough)
2) myping calculates the RTT time in milliseconds and microseconds.
<p>&nbsp;</p>
  <img width="600" height="300" src="https://www.layerstack.com/img/docs/resources/pingdiagram2.jpg">
</p>
<p>&nbsp;</p>

* PING (Packet Internet Groper) command is used to check the network connectivity between a source and destination and it use ICMP(Internet Control Message Protocol) to send  echo request messages to the destination and waiting for a response.

* ICMP is part of the Internet protocol suite as defined in RFC 792. ICMP messages are typically used for diagnostic or control purposes or generated in response to errors in IP   operations (as specified in RFC 1122). ICMP errors are directed to the source IP address of the originating packet.

The Internet Control Message Protocol (ICMP) has many messages that are identified by a "type" field, these are defined by RFCs. Many of the types of ICMP message are now obsolete and are no longer seen in the Internet. Some important ones which are widely used include:

| Type | Info |
| ----- | ---- | 
| 0 |  Echo Reply |
| 1 |  Unassigned |
| 2 |  Unassigned  |
| 3 |  Destination Unreachable |
| 4 |  Source Quench |
| 5 |  Redirect |
| 6 |   Alternate Host Address|
| 7 |   Unassigned|
| 8 |   Echo |
| 9 |   Router Advertisement |
| 10 |  Router Selection  |
| 11 | Time Exceeded |

And many more...

## Output Screenshot
![](https://i.ibb.co/SXRmX6N/icmp.png)


## sniffer.c

```c
/*****************************************************************************/
/****************************  sniffer.c   ***********************************/
/***  Requirements:                                                        ***/
/***                                                                       ***/
/***  sniffing ICMP traffic in your network and will display               ***/
/***  the following fields:                                                ***/
/***  -> IP_SRC                                                            ***/
/***  -> IP_DST                                                            ***/
/***  -> TYPE                                                              ***/
/***  -> CODE                                                              ***/
/***  For each relevant packet from your network.                          ***/
/***                                                                       ***/
/*****************************************************************************/
/*****************************************************************************/

#include <arpa/inet.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <sys/ioctl.h>
#include <net/if.h>
#include <netinet/ether.h>
#include <netinet/ip_icmp.h>
#include <netinet/ip.h>
#include <sys/socket.h>
#include <linux/if_packet.h>
#include <net/ethernet.h>

/**********************************************
 * Implementation:
 **********************************************/
int count=0;
void got_packet(unsigned char* , int );

int main(int argc, char *argv[]) {

    printf("\n######################################################\n");
    printf("   Welcome!, please wait for new ICMP packets...\n");
    printf("######################################################\n\n");

    int PACKET_LEN = IP_MAXPACKET;
    struct packet_mreq mr;

/**********************************************
 * Create the raw socket
 * -> htons(ETH_P_ALL): Capture all types of packets
 **********************************************/
    int raw_socket;
    if ((raw_socket = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL))) == -1) {
        perror("listener: socket");
        return -1;
    }

/**********************************************
 * Turn on the promiscuous mode
 **********************************************/
    mr.mr_type = PACKET_MR_PROMISC;
    setsockopt(raw_socket, SOL_PACKET, PACKET_ADD_MEMBERSHIP, &mr, sizeof(mr));

/**********************************************
 * Get captured packet
 **********************************************/
    char buffer[PACKET_LEN];
    while(1) {
        bzero(buffer,PACKET_LEN);
        int received = recvfrom(raw_socket, buffer, ETH_FRAME_LEN, 0, NULL, NULL);

        unsigned char* hex= ((unsigned char*)buffer);
        unsigned char* packet= (unsigned char *)malloc(received);

        for (int i =14,j=0; i < received-4;i++,j++) {
            packet[j]=hex[i];
        }

        got_packet(packet, received);
    }
}

void got_packet(unsigned char* buffer, int size) {

    struct iphdr *iph = (struct iphdr*)buffer;

/**********************************************
* Check if it's a ICMP protocol.
**********************************************/
    if (iph->protocol == 1) {

        unsigned short iphdrlen = iph->ihl*4;
        struct icmphdr *icmph = (struct icmphdr *)(buffer + iphdrlen);

/**********************************************
* If the IP Destination is known,
* then print the data.
**********************************************/
        unsigned int type = (unsigned int)(icmph->type);
        if(type != 96) {
            char *icmp_type_names[] = {"Echo Reply","Unassigned","Unassigned","Destination Unreachable",
                                        "Source Quench","Redirect","Alternate Host Address","Unassigned",
                                        "Echo","Router Advertisement","Router Selection","Time Exceeded"};
            struct sockaddr_in source,dest;
            memset(&source, 0, sizeof(source));
            source.sin_addr.s_addr = iph->saddr;

            memset(&dest, 0, sizeof(dest));
            dest.sin_addr.s_addr = iph->daddr;

            printf("*********************** ICMP Packet No. %d *************************\n",++count);
            printf("\nIP Header\n");
            printf("---> Source IP        : %s\n",inet_ntoa(source.sin_addr));
            printf("---> Destination IP   : %s\n",inet_ntoa(dest.sin_addr));
            printf("\nICMP Header\n");
            printf("---> Type : %d\n", (unsigned int) (icmph->type));
            printf("---> Code : %d\n", (unsigned int) (icmph->code));
            if( type <= 11) {
                printf("---> Info : %s\n",icmp_type_names[type]);
            }

        }
    }
}
/**********************************************
 * A Promiscuous Mode:
 *   is a mode for a wired network interface controller (NIC)
 *   or wireless network interface controller (WNIC) that causes the controller
 *   to pass all traffic it receives to the central processing unit (CPU) rather
 *   than passing only the frames that the controller is specifically programmed
 *   to receive.
 *   This mode is normally used for packet sniffing that takes place on a router
 *   or on a computer connected to a wired network or one being part of a wireless LAN.
 *   Interfaces are placed into promiscuous mode by software bridges often used with hardware
 *   virtualization.
 **********************************************/

/**********************************************
* A compiled BPF code:
*   if the driver for the network interface supports promiscuous mode,
*   it allows the interface to be put into that mode so that all packets
*   on the network can be received, even those destined to other hosts.
*   BPF allows a user-program to attach a filter to the socket,
*   which tells the kernel to discard unwanted packets.
**********************************************/

/**********************************************
 * Packet Capturing using raw PCAP library:
 * -> It still uses raw sockets internally,
 *    but its API is standard across all platforms.
 *    OS specifics are hidden by PCAP’s implementation.
 * -> Allows programmers to specify filtering rules using
 *    human readable Boolean expressions.
 **********************************************/
```

## myping.cpp

```cpp
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h>
#include <netinet/ip_icmp.h>
#include <arpa/inet.h>
#include <errno.h>
#include <sys/time.h>

// IPv4 header len without options
#define IP4_HDRLEN 20

// ICMP header len for echo req
#define ICMP_HDRLEN 8
#define SOURCE_IP "127.0.0.1"
// i.e the gateway or ping to google.com for their ip-address
#define DESTINATION_IP "192.168.1.1"

/*****************************************************************************/
/****************************  myping.cpp  ***********************************/
/***  Requirements:                                                        ***/
/***                                                                       ***/
/***  1) myping.cpp sends ICMP ECHO REQUEST and receives                   ***/
/***     ICMP-ECHO-REPLY (one time is enough).                             ***/
/***                                                                       ***/
/***  2) myping.cpp calculates the RTT time in milliseconds and            ***/
/***     microseconds.                                                     ***/
/***                                                                       ***/
/*****************************************************************************/
/*****************************************************************************/

// Checksum algorithm
unsigned short calculate_checksum(unsigned short * paddress, int len);

int main() {
/*--------------------------------------------------------------------*/
/*--- ICMP header                                                  ---*/
/*--------------------------------------------------------------------*/
    struct icmp icmphdr; // ICMP-header

    // Message Type (8 bits): ICMP_ECHO_REQUEST
    icmphdr.icmp_type = ICMP_ECHO;

    // Message Code (8 bits): echo request
    icmphdr.icmp_code = 0;

    // Identifier (16 bits): some number to trace the response.
    // It will be copied to the response packet and used to map response to the request sent earlier.
    // Thus, it serves as a Transaction-ID when we need to make "ping"
    icmphdr.icmp_id = 18;

    // Sequence Number (16 bits): starts at 0
    icmphdr.icmp_seq = 0;

    // ICMP header checksum (16 bits): set to 0 not to include into checksum calculation
    icmphdr.icmp_cksum = 0;

    // Combine the packet
    char packet[IP_MAXPACKET];

    // ICMP header.
    memcpy (packet, &icmphdr, ICMP_HDRLEN);

    char data[IP_MAXPACKET] = "This is the ping.\n";
    int datalen = strlen(data) + 1;

    // ICMP data.
    memcpy (packet + ICMP_HDRLEN, data, datalen);

    // Calculate the ICMP header checksum
    icmphdr.icmp_cksum = calculate_checksum((unsigned short *) packet, ICMP_HDRLEN + datalen);
    memcpy (packet, &icmphdr, ICMP_HDRLEN);

/*--------------------------------------------------------------------*/
/*--- Create Raw Socket                                            ---*/
/*--------------------------------------------------------------------*/
    struct sockaddr_in dest_in;
    memset(&dest_in, 0, sizeof (struct sockaddr_in));
    dest_in.sin_family = AF_INET;

    // The port is irrelant for Networking and therefore was zeroed.
    dest_in.sin_addr.s_addr = inet_addr(DESTINATION_IP);

    // Create raw socket for IP-ICMP
    int sock = -1;
    if ((sock = socket (AF_INET, SOCK_RAW, IPPROTO_ICMP)) == -1) {
        fprintf (stderr, "socket() failed with error: %d", errno);
        fprintf (stderr, "To create a raw socket, the process needs to be run by Admin/root user (sudo).\n\n");
        return -1;
    }

/*--------------------------------------------------------------------*/
/*--- Send the ICMP ECHO REQUEST packet                            ---*/
/*--------------------------------------------------------------------*/

    struct timeval start, end;
    gettimeofday(&start, NULL);

    // Send the packet using sendto() for sending Datagrams.
    int sent_size  = sendto(sock, packet,  ICMP_HDRLEN + datalen, 0, (struct sockaddr *) &dest_in, sizeof (dest_in));
    if (sent_size == -1) {
        fprintf (stderr, "sendto() failed with error: %d", errno);
        return -1;
    }
    printf("\nSent one packet. \nsent: %d \n\n",sent_size);

/*--------------------------------------------------------------------*/
/*--- Receive the ICMP ECHO REPLY packet                           ---*/
/*--------------------------------------------------------------------*/
    bzero(packet,IP_MAXPACKET);
    socklen_t len = sizeof(dest_in);
    int get_size = -1;
    while (1) {
        get_size = recvfrom(sock, packet, sizeof(packet), 0, (struct sockaddr *) &dest_in, &len);
        if (get_size > 0) {
            printf("Get one packet.\nget: %d (+ IP4_HDRLEN) \n\n",get_size);
            break;
        }
    }

    gettimeofday(&end, NULL);

    char reply[IP_MAXPACKET];
    memcpy(reply, packet + ICMP_HDRLEN + IP4_HDRLEN, datalen);
    printf("ICMP reply: %s \n", reply);

    // second > milliseconds (10^(-3) seconds) > microseconds (10^(-6) seconds)
    float milliseconds = (end.tv_sec - start.tv_sec) * 1000.0f + (end.tv_usec - start.tv_usec) / 1000.0f;
    unsigned long microseconds = (end.tv_sec - start.tv_sec) * 1000.0f + (end.tv_usec - start.tv_usec);
    printf("RTT time in milliseconds: %f \n", milliseconds);
    printf("RTT time in microseconds: %ld\n\n", microseconds );

    // Close the raw socket descriptor.
    close(sock);

    return 0;
}

/*--------------------------------------------------------------------*/
/*--- Checksum - Calculate the ICMP header checksum                ---*/
/*--------------------------------------------------------------------*/
unsigned short calculate_checksum(unsigned short * paddress, int len) {
    int nleft = len;
    int sum = 0;
    unsigned short * w = paddress;
    unsigned short answer = 0;

    while (nleft > 1) {
        sum += *w++;
        nleft -= 2;
    }

    if (nleft == 1)
    {
        *((unsigned char *)&answer) = *((unsigned char *)w);
        sum += answer;
    }

    // add back carry outs from top 16 bits to low 16 bits
    sum = (sum >> 16) + (sum & 0xffff); // add hi 16 to low 16
    sum += (sum >> 16);                 // add carry
    answer = ~sum;                      // truncate to 16 bits

    return answer;
}

```
