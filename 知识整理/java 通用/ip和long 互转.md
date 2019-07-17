// <summary>
        ///  将127.0.0.1形式的IP地址转换成十进制整数
        /// </summary>
        /// <param name="strIp"></param>
        /// <returns></returns>
        public long IpToLong(string strIp)
        {
            long[] ip = new long[4];
            int position1 = strIp.IndexOf(".");
            int position2 = strIp.IndexOf(".", position1 + 1);
            int position3 = strIp.IndexOf(".", position2 + 1);
            // 将每个.之间的字符串转换成整型  
            ip[0] = long.Parse(strIp.Substring(0, position1));
            ip[1] = long.Parse(strIp.Substring(position1 + 1, position2 - position1 - 1));
            ip[2] = long.Parse(strIp.Substring(position2 + 1, position3 - position2 - 1));
            ip[3] = long.Parse(strIp.Substring(position3 + 1));
            //进行左移位处理
            return (ip[0] << 24) + (ip[1] << 16) + (ip[2] << 8) + ip[3];
        }

        /// <summary>
        /// 将十进制整数形式转换成127.0.0.1形式的ip地址 
        /// </summary>
        /// <param name="ip"></param>
        /// <returns></returns>
        public string LongToIp(long ip)
        {
            StringBuilder sb = new StringBuilder();
            //直接右移24位
            sb.Append(ip >> 24);
            sb.Append(".");
            //将高8位置0，然后右移16
            sb.Append((ip & 0x00FFFFFF) >> 16);
            sb.Append(".");
            //将高16位置0，然后右移8位
            sb.Append((ip & 0x0000FFFF) >> 8);
            sb.Append(".");
            //将高24位置0
            sb.Append((ip & 0x000000FF));
            return sb.ToString();
        }